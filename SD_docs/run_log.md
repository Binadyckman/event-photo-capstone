# Run Log

## node --version
- Result: v24.21.0  
- Worked: yes 

## npm install
- Result: added 160 packages, audited 161 packages in 26s
- Worked: yes, no errors

## npm test
- Result: 1 test file, 5 tests passed, 0 failed (duration 1.28s, Vitest v3.2.7)
- Worked: yes
- Tests that ran (file: tests/event-api.test.ts):
  1. requires a demo photographer identity
  2. lists only events belonging to the requested photographer
  3. does not return another photographer's event
  4. allows a client to submit a selection for their own gallery
  5. does not let a client access a different client's gallery
- Verified: the 5 tests pass on this computer
- Not verified: what exactly each test checks inside
## npm run typecheck
- Result: `tsc --noEmit` finished with no errors printed
- Worked: yes

## npm run dev
- Result: "Event Photo API listening on http://localhost:3000"
- Worked: yes, no errors

## curl checks (server running)
- `curl.exe http://localhost:3000/health` returned {"status":"ok"}
- `curl.exe -H "x-photographer-id: studio-amber" http://localhost:3000/api/events` returned [{"id":"event-100","title":"Cohen Wedding","galleryPublished":true}]
- `curl.exe -H "x-photographer-id: studio-amber" http://localhost:3000/api/events/event-100/gallery` returned {"id":"event-100","title":"Cohen Wedding","photos":["photo-1","photo-2","photo-3"]}
## npm run typecheck
- Result: `tsc --noEmit` finished with no errors printed
- Worked: yes

## npm run dev
- Result: "Event Photo API listening on http://localhost:3000"
- Worked: yes, no errors

## curl checks (server running)
- `curl.exe http://localhost:3000/health` returned {"status":"ok"}
- `curl.exe -H "x-photographer-id: studio-amber" http://localhost:3000/api/events` returned [{"id":"event-100","title":"Cohen Wedding","galleryPublished":true}]
- `curl.exe -H "x-photographer-id: studio-amber" http://localhost:3000/api/events/event-100/gallery` returned {"id":"event-100","title":"Cohen Wedding","photos":["photo-1","photo-2","photo-3"]}
- Not verified: the client gallery