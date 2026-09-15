What it does

GET /api/todos returns all todos, same as before.
GET /api/todos?done=true returns only the done ones.
GET /api/todos?done=false returns only the pending ones.

The UI has three buttons — All, Active, Done — that switch between them.

Changes

backend/controllers/todoController.js
getTodos reads done from req.query and builds a filter object. If there's no done param the object stays empty, so all todos come back.

I compare against the strings 'true' and 'false' because query params are always strings. Using if (done) would break, since 'false' is truthy.

frontend/src/api/todos.js
fetchTodos now takes an optional argument and sends it as a query param. Axios ignores undefined, so calling it with nothing still sends a plain request.

frontend/src/App.jsx
Added filter state ('all', 'active', 'done') and put it in the useEffect dependency array so the list re-fetches when you switch tabs. Added the three buttons above the list.

Nothing else was changed — no new route was needed.

Server vs client filtering

I did it server-side, as the task asked.

Client-side would be faster to switch tabs and needs only one request, but it sends every todo to the browser and breaks once you add pagination, since it can only filter what's already loaded.

For a small list either works.
