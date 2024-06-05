### the yaml file to push an Docker Image to the GCP Artifact Repository

name: Deploy nginx

on:
push:
branches: - "main"

jobs:
deploy:
runs-on: ubuntu-latest
steps: - name: Code Checkout
uses: actions/checkout@v2

      - name: Installl the gcloud cli
        uses: google-github-actions/setup-gcloud@v0
        with:
          project_id: ${{ secrets.GOOGLE_PROJECT }}
          service_account_key: ${{ secrets.GOOGLE_APPLICATION_CREDENTIALS }}
          export_default_credentials: true

      - name: build and push Docker Image
        env:
          GOOGLE_PROJECT: ${{ secrets.GOOGLE_PROJECT }}
        run: |
          # Configuring docker with gcloud configuration
          gcloud auth configure-docker us-central1-docker.pkg.dev
          # Run our Build (Copy Build path from the Artifact Registry i.e us-central1-docker.pkg.dev/natural-system-425507-v9/demo)
          #  With the name `nginx:latest` and adding the current directory "."
          docker build -t us-central1-docker.pkg.dev/natural-system-425507-v9/demo/nginx:latest .
          # push that image
          docker push us-central1-docker.pkg.dev/natural-system-425507-v9/demo/nginx:latest
