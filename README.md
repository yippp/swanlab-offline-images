# SwanLab Offline Images Packager

This repository uses GitHub Actions to pull the official SwanLab self-hosted Docker images, package them into a single `swanlab_images.tar.gz`, and upload it to a Hugging Face Dataset.

It is designed for enterprise intranet / offline / air-gapped environments (including Alibaba Cloud private Feitian / Apsara Stack) where you cannot directly pull images from the internet.

## Usage

1. Add a repository secret named `HF_TOKEN` (Hugging Face token with write permission to datasets).
2. Go to **Actions** → **Package SwanLab Images & Upload to Hugging Face** → **Run workflow**.
3. Fill in:
   - `hf_repo_id`: Your Hugging Face dataset (e.g. `yippp/swanlab-offline-images`)
   - `swanlab_version`: Version tag (default `v3.6.4`)
   - Optionally set private flag.
4. After the workflow finishes, download the `swanlab_images-*.tar.gz` from the Hugging Face dataset.

## On the target offline server

```bash
# Load the images
docker load -i swanlab_images-v3.6.4.tar.gz

# Then clone the official self-hosted repo and run the install script
git clone https://github.com/SwanHubX/self-hosted.git
cd self-hosted/docker
./install.sh
```

See the official SwanLab offline deployment docs for full details:
https://docs.swanlab.cn/self_host/docker/offline-deployment.html

## Notes

- Images are pulled from `repo.swanlab.cn` (China mirror) by default.
- The package contains all required images for the Docker Compose deployment.
- Large file: expect several GB. Hugging Face Datasets supports large files well.
- Inspired by [llm-docker-builder](https://github.com/yippp/llm-docker-builder).
