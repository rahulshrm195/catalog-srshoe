# One-time Firebase setup

Steps that can't be done from this repo; run them once in the Firebase / Google Cloud console.

## 1. Let the website read the published catalog file (CORS)

Without this, browsers block `public/catalog.json`, and every visitor falls back to
reading every product straight from Firestore (slow, and ~1,600 reads per visit).

1. Open https://console.cloud.google.com/ for project **sr-shoes** and click the
   **Cloud Shell** icon (`>_`, top right).
2. In Cloud Shell, upload `cors.json` from this repo (⋮ menu → Upload), then run:

   ```sh
   gsutil cors set cors.json gs://sr-shoes.firebasestorage.app
   gsutil cors get gs://sr-shoes.firebasestorage.app   # should print the config back
   ```

3. Open the catalog, then in the browser's developer console there should be no
   "blocked by CORS policy" error.

## 2. Make existing photos load from the phone's cache

Photos uploaded before v1.4.0 were stored with "don't cache", so phones re-download
every photo on every visit. New uploads are cached automatically. To fix the old ones:

Admin → **Images** tab → **Speed up photo loading**. It goes through every photo once
(about a minute for ~1,000 photos) and is safe to run again.
