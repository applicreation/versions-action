# versions-action

## Usage

```yaml
on:
  release:
    types:
      - published
jobs:
  release:
    runs-on: ubuntu-latest
    steps:  
      - uses: applicreation/versions-action@v2
        id: versions
        with:
          version: ${{ github.ref }}
      - run: echo ${{ steps.versions.outputs.patch }}
      - run: echo ${{ steps.versions.outputs.minor }}
      - run: echo ${{ steps.versions.outputs.major }}
```

## Example

```yaml
---

name: Docker release
on:
  release:
    types:
      - published
jobs:
  docker-release:
    name: Build and push
    runs-on: ubuntu-latest
    steps:
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v2
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      - name: Login to GitHub Container registry
        uses: docker/login-action@v2
        with:
          registry: ghcr.io
          username: ${{ github.repository_owner }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - name: Get versions
        uses: applicreation/versions-action@v2
        id: versions
        with:
          version: ${{ github.ref }}
      - name: Build and push
        uses: docker/build-push-action@v3
        with:
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:${{ steps.versions.outputs.patch }}
            ghcr.io/${{ github.repository }}:${{ steps.versions.outputs.minor }}
            ghcr.io/${{ github.repository }}:${{ steps.versions.outputs.major }}
            ghcr.io/${{ github.repository }}:latest
```
