# TimeTailor

TimeTailor is a student day-planning prototype based on the approved TRF. It packs flexible tasks around fixed class and work commitments, considers locations and transit buffers, and offers a delay rescheduler and baseline comparison.

## Run

Open `index.html` through a local web server (for example, VS Code Live Server). Firebase Authentication and Realtime Database must be enabled for the Firebase project configured in `index.html`; the Realtime Database rules must allow each authenticated student to read and write their own `tasks/{uid}` and `users/{uid}` records.

## Google Maps transit times

To enable route-based travel estimates, set `GOOGLE_MAPS_API_KEY` in `index.html` to a browser key for a Google Cloud project with Maps JavaScript API, Places API (New), and Routes API enabled. Restrict the key to the deployed site's HTTP referrers and the required APIs, and set an appropriate quota/billing limit. The app resolves entered locations with Places Text Search, then requests transit route matrices in batches. Without a configured key (or when the route service is unavailable), it clearly labels the 15-minute different-location buffer as an estimate. The Google Routes matrix has a 100-element limit for transit requests.

## Scheduling behavior

- Fixed commitments are treated as timetable anchors. The engine reports overlaps, out-of-day anchors, impossible anchor-to-anchor transit, and tasks it cannot fit.
- Flexible tasks have a priority, deadline, earliest available time, and latest start. The hybrid engine splits the day at fixed anchors, uses a 15-minute-unit dynamic program to select a feasible subset in each gap, then greedily orders and places the selected tasks.
- The schedule inserts a 15-minute rest break after 180 minutes of scheduled flexible work. Transit buffers are displayed as separate timeline entries.
- The delay action adds 30 minutes to the first fixed anchor and rebuilds the schedule. The benchmark compares the hybrid engine with Priority-First and Earliest Deadline First under the same fixed commitments and transit model.

## TRF scope notes

This is a client-side prototype backed by Firebase. Task deadlines are time-of-day values for the selected schedule date. Travel uses Google Routes transit estimates when configured; otherwise it uses the labeled fallback estimate. The current comparison reports conflicts, missed tasks/deadlines, estimated travel time, idle time, and packing rate. It does not yet represent a full multi-day academic calendar or institutional UAT results.
