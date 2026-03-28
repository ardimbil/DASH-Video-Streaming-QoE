DASH Video Streaming and QoE Evaluation Project

TThis project demonstrates an end-to-end implementation of Dynamic Adaptive Streaming over HTTP (DASH) using a client-server architecture with Linux virtual machines. The goal was to see how different network conditions affect video quality and to evaluate the viewing experience using Mean Opinion Score (MOS).

---

🎯 Objectives/ What This Project Does

* Transcodes HD videos into multiple bitrates (1.5 Mbps, 2 Mbps, 4 Mbps)
* Generates DASH manifests and segments using FFmpeg
* Hosts the streams on an Apache web server
* Plays back the video using a DASH-compatible player
* Simulates network impairments with Linux Traffic Control (TBF, HTB, policing)
* Measures Quality of Experience (QoE) using MOS

---

🛠️ Tools and Technologies

* Ubuntu Linux (VM)
* FFmpeg
* Apache Web Server
* iPerf3
* Linux Traffic Control (tc)
* DASH Player (Bitmovin)

---

📂 Project Structure

```
DASH-Streaming-Project/
│
├── report.pdf
├── README.md
├── commands.txt
├── manifest.mpd
├── videos/
├── segments/
├── screenshots/
│   ├── vm_setup.png
│   ├── ffmpeg_install.png
│   ├── transcoding.png
│   ├── dash_files.png
│   ├── web_server.png
│   ├── playback.png
│   ├── iperf.png
│   ├── tbf.png
│   ├── htb.png
│   ├── policing.png
```

---

⚙️ Setup Instructions

1. Install FFmpeg

```bash
sudo apt update
sudo apt install ffmpeg
```

2. Transcode Video

```bash
ffmpeg -i input.mp4 -b:v 1500k output_1.5.mp4
```

3. Generate DASH Manifest

```bash
ffmpeg -i input.mp4 -map 0 -b:v:0 1500k -b:v:1 2000k -b:v:2 4000k -f dash manifest.mpd
```

 4. Run Web Server

```bash
sudo apt install apache2
```

Access video:

```
http://<server-ip>/manifest.mpd
```

---

 🌐 Network Scenarios

 1. TBF (Traffic Shaping)

```bash
tc qdisc add dev enp0s8 root tbf rate 2.5mbit burst 20k latency 50ms
```

2. HTB (Hierarchical Control)

```bash
tc qdisc add dev enp0s8 root handle 1: htb default 10
tc class add dev enp0s8 parent 1: classid 1:10 htb rate 2.5mbit ceil 5mbit burst 20k
```

3. Traffic Policing

```bash
tc qdisc add dev enp0s8 handle ffff: ingress
tc filter add dev enp0s8 parent ffff: protocol ip u32 match u32 0 0 police rate 3.5mbit drop
```

---



| Scenario | MOS |
| -------- | --- |
| Normal   | 5   |
| TBF      | 3.5 |
| HTB      | 4   |
| Policing | 2   |

---



* Bandwidth limitation reduces video quality
* HTB provides better QoE than TBF
* Packet loss (policing) severely impacts QoE
* Adaptive streaming improves resilience

---


Reference

Included in the report (IEEE format)

---

## 👨‍💻 Author

Abdul-Rahman Dimbil 
