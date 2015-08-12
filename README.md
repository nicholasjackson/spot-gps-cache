# spot-gps-cache
Simple server written in Go to cache calls to Spot GPS Tracker API.

The spot GPS Tracker API allows you to retrieve the current location, however the API has a restriction which only allows one call to be made every 2.5 minutes.

This simple API makes the calls to the API on your behalf and caches the response and controlling the 2.5 minute limit allowing your web applications to retreive the current locations as often as required without danger of being banned by Spot.
