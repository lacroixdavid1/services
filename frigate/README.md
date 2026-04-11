# frigate

NVR (Network Video Recorder) with real-time object detection.

- **Website:** https://frigate.video
- **Docs:** https://docs.frigate.video
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `uptime_kuma` | Health monitoring |

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY` | Tailscale auth key |
| `FRIGATE_RTSP_USER` | RTSP stream username for cameras |
| `FRIGATE_RTSP_PASS` | RTSP stream password for cameras |
| `FRIGATE_UNIFI_HOST` | UniFi Protect host for camera integration |
| `DATA_NVME_PATH` | Path to NVMe storage for config and state |
| `DATA_PATH` | Path to HDD storage for recordings and clips |

## Notes

- AMD GPU passed through for hardware-accelerated detection via ROCm (`frigate:rocm` image)
- Camera streams configured in `config/config.yml` using `${FRIGATE_RTSP_USER}` and `${FRIGATE_RTSP_PASS}`
- UniFi Protect integration uses `${FRIGATE_UNIFI_HOST}`
- 256 MB shared memory allocated for video processing (`/dev/shm`)
- Recordings and clips stored on HDD (`${DATA_PATH}`)
