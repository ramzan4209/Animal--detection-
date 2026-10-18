# Animal--detection-
For the website 
# Two-site Animal Detection Demo

Phone 1 uploads an image on the uploader website. A FastAPI cloud backend runs YOLO, stores the latest detection in memory, and returns an annotated image. Phone 2 opens the separate dashboard website, which polls the backend every three seconds.

## Files
- `backend/`: FastAPI + YOLO API
- `uploader-site/`: separate static website for uploading photos
- `dashboard-site/`: separate static website for monitoring results

## 1. Deploy the API to Render
1. Create a GitHub repository and upload the contents of this project. Keep `main.py`, `requirements.txt`, and `Procfile` together in the repository root OR deploy the `backend` folder as the Render root directory.
2. On Render, create a **Web Service** connected to the repo.
3. If the repo root is the project root, set **Root Directory** to `backend`.
4. Build command: `pip install -r requirements.txt`
5. Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
6. Deploy. The first deployment downloads YOLO weights and can take several minutes. Free services may sleep or run out of resources.
7. Copy the service URL, such as `https://your-animal-api.onrender.com`.
8. Test `https://YOUR-API-URL/health` in a browser. It should return JSON with `"status":"ok"`.

## 2. Deploy the uploader website
1. Deploy the contents of `uploader-site` as a static site using Netlify or another static hosting provider.
2. Open its public URL on Phone 1.
3. Paste the Render API URL into **Cloud API URL**, then tap **Save API URL**.
4. Choose an elephant image and tap **Upload and detect animal**.

## 3. Deploy the dashboard website separately
1. Create a second static site using the contents of `dashboard-site`.
2. Open its public URL on Phone 2.
3. Paste the same Render API URL and tap **Save API URL**.
4. Keep the dashboard open. It checks for the newest result every three seconds.

## 4. Quick test
- Open `/health` on the API URL.
- Upload an image on the uploader website.
- Confirm the result appears on Phone 1.
- Check Phone 2 dashboard for the latest image and report.

## Important prototype limitations
- The backend stores only the latest result in process memory. Results are lost if Render restarts or sleeps. Use a database and object storage for a production version.
- The endpoint is public and has no login/rate limit. Use only test images; add authentication and rate limiting before real deployment.
- The default YOLO model recognizes only its supported classes, including elephant, dog, cat, bird, cow, sheep, horse, bear, zebra and giraffe. It will not recognize every animal species.
- CORS is open for simple testing. For production, change `allow_origins=["*"]` to your exact uploader and dashboard URLs.
- Use an API host with enough RAM/CPU for Ultralytics/PyTorch. Free hosting limits may prevent this model from running reliably; if deployment fails due to memory, test locally or use a hosted inference API.

- 
