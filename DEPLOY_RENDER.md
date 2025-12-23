Deploying to Render (Django)

This project is prepared to be deployed on Render.com using Gunicorn and WhiteNoise for static file serving.

Steps:
1. Ensure `requirements.txt` includes necessary runtime packages (already present):
   - Django==6.0
   - gunicorn
   - whitenoise
   - pillow
   - razorpay

2. Add Render service:
   - In the Render dashboard, create a new Web Service and connect your GitHub repo.
   - Set the Build Command: pip install -r requirements.txt
   - Set the Start Command (or use the Procfile): gunicorn core.wsgi:application --bind 0.0.0.0:$PORT

3. Environment variables (add in Render):
   - RAZORPAY_KEY_ID
   - RAZORPAY_KEY_SECRET
   - SECRET_KEY (a strong Django SECRET_KEY)
   - DEBUG should be set to `False` in production (override via env var handling or settings)

4. Static files:
   - Render runs `collectstatic` during build if you add it to post-build or ensure Start Command runs `python manage.py collectstatic --noinput` before starting gunicorn.
   - WhiteNoise is configured in `core/settings.py` and will serve static files from `STATIC_ROOT`.

5. Media files:
   - For production, configure a persistent storage for media files (S3, Google Cloud Storage, or Render Persistent Disks). The current setup serves media via Django (development only) and is NOT suitable for production.

6. After deployment, verify:
   - Static assets load (CSS, JS)
   - Uploads (if using external storage) and Razorpay payments work

Notes
- The repo includes a `Procfile` and `requirements.txt` to make Render setup straightforward.
- If you need help configuring S3 or another media storage provider, I can add Django-Storages and the necessary settings.
