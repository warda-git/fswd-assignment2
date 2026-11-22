# Acceptance and viva checklist

Use Postman and capture real screenshots. Restart server for a clean state where needed.

- [ ] Register REQUESTER (201), login (200), duplicate normalized email (409), wrong password (401), registration with `role` or `id` (400).
- [ ] Missing JWT (401), invalid JWT (401), expired JWT (401; set `JWT_EXPIRES_IN=1s`, restart, log in and wait), dispatcher attempted to create a request (403), requester accesses someone else's ID (403).
- [ ] Login as Aisha and create IT request; dispatcher assigns Sara; Sara starts and resolves; Aisha closes, sends feedback; inspect history and dashboard.
- [ ] Description shorter than 20 chars (400), `randomField` in body (400), `/requests/abc` (400), priority `URGENT` (400), `limit=51` (400), `page=1.5` (400).
- [ ] Requester attempts sixth active request (409).
- [ ] Dispatcher assigns fourth active job to same technician (409), assigns unsupported category (409).
- [ ] Technician resolves before starting (409), unassigned technician starts (403), requester edits/cancels after assignment (409).
- [ ] Aisha reopens a RESOLVED request with 10+ character reason; verify assignment becomes null and old history remains. Check active limits on reopen.
- [ ] Feedback on OPEN/RESOLVED request (409), feedback twice (409); dispatcher dashboard shows average and feedback count, including final technician after reassign on reopen.
- [ ] `GET /requests?search=...&categoryId=...&status=...&priority=...&page=1&limit=2`; compare scoped totals across Aisha/Ali/dispatcher.
- [ ] Dispatcher deactivates Furniture; cannot create new request for it (400), existing requests persist. Restart server and verify reset.
- [ ] Successful requests return `X-Request-Id`, `success:true` and data. Failed requests retain 400/401 errors. Observe `LIFECYCLE` console labels and final request logs.
- [ ] `npm test` passes fake storage replacement.
- [ ] Competing concurrent creations/assignments at capacity: use Postman Runner or `Promise.all` against a near-limit state; exactly allowed number succeeds, others fail 409, and unsuccessful operations do not add history.

## Easy viva changes

- Request maximum 5 -> 3: change the count in `RequestsService.create()` (and describe updated tests).
- Technician capacity 3 -> 2: change `RequestsService.assign()`.
- Resolution note minimum 20 -> 30: change `@Length(20,1000)` in `ResolveDto`.
- Add priority enum value: edit `Priority` in `models.ts`, then show DTO enum validation.
- Modify default pagination: change `ListDto` defaults.
- Change how returned JSON looks: `WrapInterceptor` in `common.ts`.
- Explain dependency injection: `@Inject(FIXFLOW_STORE)` in services, binding in `StorageModule`.
