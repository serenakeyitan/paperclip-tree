# Issue Comment Cancellation

Individual issue comments can be fetched and cancelled (deleted) through dedicated endpoints: `GET /issues/:id/comments/:commentId` and `DELETE /issues/:id/comments/:commentId`. Cancellation removes the comment row and bumps the parent issue's `updatedAt`, returning the redacted deleted comment to the caller.

## Key Decisions

### Cancel Is a Hard Delete With Audit Trail

The service-layer `removeComment` performs a hard delete inside a transaction and updates the parent issue timestamp so live update subscribers see the change. Activity logging is performed by the route layer rather than the service so the actor (user or agent) is captured at the API boundary.

### Queued Comments For Active Runs Are Special-Cased

The routes layer uses `isQueuedIssueCommentForActiveRun` to distinguish comments that are still queued for an in-flight heartbeat run from those that have already been consumed. This determines whether cancellation should also unblock or interact with the active run rather than only deleting the row.

### Frontend Surfaces Cancel via `issuesApi.cancelComment`

The UI exposes cancellation through `issuesApi.cancelComment(id, commentId)` and fetch-by-id via `issuesApi.getComment`, enabling optimistic comment management in the issue thread.
