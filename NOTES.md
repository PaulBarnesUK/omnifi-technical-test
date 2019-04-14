# Notes

## Fetch
---

I opted to use fetch as opposed to any 3rd party HTTP library as, based upon the task specification, we only need to support modern browsers, and this will save on the file size.

## API location coordinates incorrect
---

Some of the API location coordinates seem to be incorrect, with United Kingdom showing as being an underwater country!
I attempted to use Google's Geocode library to recreate the coordinates based on the location's name but the API key does not have access to this library. For the purposes of this test this is obviously not an issue, but in a production application I would take further steps to ensure the data is correct.

## Development URL
---

A development URL for this project is available at https://sharp-bhaskara-236b0b.netlify.com/