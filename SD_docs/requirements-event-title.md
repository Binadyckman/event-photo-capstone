## Rename event title

**Student:** Bina Dyckman  **Date:** 24/10/2026  **Fork URL:** https://github.com/Binadyckman/event-photo-capstone.git

## 1. Original request (do not reinterpret)
> I entered the wrong event name. I want to be able to change the event name displayed in the client's gallery.

## 2. Current behavior — verified facts
| Claim | Evidence: file/function/line or HTTP result | Verified? |
| --- | --- | --- |
| Event title exists in data model | `src/domain/types.ts`, type `EventRecord`: `title: string` | Yes |
| Current client gallery presents title | Photographer gallery: I ran `curl.exe -H "x-photographer-id: studio-amber" http://localhost:3000/api/events/event-100/gallery` and got `{"id":"event-100","title":"Cohen Wedding","photos":["photo-1","photo-2","photo-3"]}`. The client route `/client/events/:eventId/gallery` exists in `app.ts` | Photographer gallery yes, client gallery not run |
| No rename endpoint exists in inspected route file | `src/http/app.ts` has 5 routes: GET /health, GET /api/events, GET /api/events/:eventId/gallery, GET /client/events/:eventId/gallery, POST /api/events/:eventId/selections. None change an event or its title. | Yes |


## 3. Actors / users / use cases / permissions
- Verified:
  - Each event has one photographer (`photographerId`) and one client (`clientId`) (`types.ts`).
  - A photographer finds an event only if the event id and his id match. A client finds an event only if the event id and his id match (`memory-store.ts`).
  - The identity headers `x-photographer-id` and `x-client-id` are only a demo, not real login (`app.ts` comment, README limitation 1).
- Must clarify:
  - Who is allowed to rename: only the photographer, or also an admin or the client?
  - How the system knows a person is allowed to rename, since there is no real login.
  - If a title can still be changed after the gallery was published and shared.
  - What counts as a valid title (empty, maximum length, characters).
## 4. Goal, scope, out of scope
- Goal: a photographer can fix the title of his event, and the client sees the new title in his gallery.
- In scope (confirmed): fixing the title of an event that already exists, and showing the new title in the client's gallery.
- Out of scope (confirmed): photo selections, service options, AI editing tools, other event details, screen design, choosing an API, writing code.
- Undecided: if the title also appears anywhere else.

## 5. Functional requirements
- FR-1 (provisional): A photographer can change the title of his own event.
- FR-2 (provisional): After the change, the client's gallery for that event shows the new title.
- FR-3 : A photographer cannot change the title of another photographer's event.

## 6. Non-functional requirements / constraints
- NFR-1 (security): Photographers must stay separated.
- NFR-2 (unresolved): Rules for a valid title (empty, length, characters). There is no validation.
- NFR-3 (security): Only a person who is really allowed to rename an event may change its title. now the identity headers are only a demo, not a real login (`app.ts`), so the system cannot know who is really asking. 


## 7. Ten analysis lenses from the preparation assignment
| Term | Question relevant to this change | Answer known? / evidence |
| --- | --- | --- |
| functional requirements | Does the change replace the title everywhere at once? | Not known |
| non-functional requirements | Must the new title still be there after a restart? | No, with memory storage it is lost (`memory-store.ts`) |
| actors / users | Who can rename: photographer, admin, client? | Photographer is my guess, not verified |
| use cases | Can the title be fixed after the client already got the gallery? Does he get a message? | Not known |
| constraints | Is there a maximum length or limit on characters? | in `title` is just a string |
| assumptions | Is the title used anywhere else? execpt for the 2 galaries| Not known. |
| acceptance criteria | How fast will the client see the new title? | Not known |
| ambiguity | What does "the event name displayed in the client's gallery" include? | Not clear |
| missing information | Do we need to keep old titles? | Not known |
| scope | Only the title, or other event details too? | Not known|

## 8. Questions, assumptions, ambiguities
| Question or assumption | Why it matters | Evidence/status | Who decides? |
| --- | --- | --- | --- |
| Who is allowed to rename an event? | We need to know who can rename an event, so we can write the permission rule and test it | Open. (`app.ts`) | Product owner |
| What are the title rules (empty, length)? | Decides what is rejected | Open | Product owner |
| Must the new title survive a restart? | Memory storage loses it | Open (`memory-store.ts`) | Product owner |
| Assumption: the client gallery shows the same title as the photographer gallery | The whole request depends on it | Not verified.|  
| Is the title shown anywhere else? | Decides how many places will change | Open | Product owner |
## 9. Acceptance criteria (provisional if decisions unresolved)
- Given an existing event and the photographer who owns it, when he changes the title to a valid title, then the client's gallery for that event shows the new title. (Provisional: permission and title rules are not decided.)
- Given an event that belongs to photographer A, when photographer B tries to change its title, then the title stays the same and the request is refused.
- Given an event with a fixed title, when the photographer opens his own gallery for it, then he also sees the new title. 

## 10. Error cases and tests
- Unknown event: the request should be refused. now the galleries return 404 "Event not found" / "Gallery not found" (`app.ts`), so I expect the same here.
- Unauthorized requester: the title must not change. now missing identity returns 401 (`app.ts`). What counts as "not allowed" is still open.
- Empty title: should be rejected, but the exact rules are not decided.
- Existing client gallery after change: should show the new title.

## 11. AI checkpoints: understanding / questioning / spec review
Record one claim you verified in source and at least two AI suggestions with adopted/rejected/open status.

I got these suggestions from Claude while working on this task. 

### AI question and spec review suggestions — reviewed critically
| AI suggestion | Essential / later / irrelevant / unverified | Justification | Evidence |
| --- | --- | --- | --- |
| One photographer must not change another photographer's event | Essential | The code and the tests already keep photographers separate. A rename must keep that. | `memory-store.ts`; test "does not return another photographer's event" |
| Ask who is allowed to rename | Essential | There is no real login, only demo headers, so it is not clear who is allowed. | `app.ts` |
| Keep a history of old titles (open) | Later | Nothing in the code or the request says we need it. Someone has to decide. | Not in the code.|

## 12. Remaining decisions (do not silently invent answers)
- Who may rename an event.
- The rules for a valid title.
- If the new title must stay after a restart.
- Where else the title appears.
- If old titles must be kept.

## 13. Personal reflection
- One fact discovered through code review:  "I saw in app.ts that there is no route that changes an event, so right now a title cannot be fixed."
- One AI suggestion you rejected or postponed, and why: "I postponed the history of old titles, because nothing in the code or the request says we need it."

