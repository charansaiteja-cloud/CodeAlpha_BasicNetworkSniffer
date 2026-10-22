# CodeAlpha Basic Network Sniffer

A beginner-friendly Python 3 network sniffer using Scapy for independent Python network packet analysis project.

## Features

- Select an authorized network interface.
- Capture a bounded number of packets or stop continuous capture with Ctrl+C.
- Identify IPv4, IPv6, ARP, TCP, UDP, ICMP, and DNS when decoded by Scapy.
- Display timestamps, addresses, ports, protocols, and packet lengths.
- Export packet metadata to CSV using Python's CSV writer.
- Keep payload previews disabled by default and restrict optional previews to ICMP traffic.
- Test packet parsing offline using pytest and synthetic packets.

## Requirements

- Kali Linux and Python 3
- Git
- Scapy and pytest

## Installation

From the project directory:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Interface discovery

```bash
ip -brief link
ip -brief address
ip route
python sniffer.py --list-interfaces
```

Use only interfaces you own or are authorized to monitor.

## Usage

Capture at most five packets:

```bash
python sniffer.py -i wlan0 -c 5 -o demo.csv
```

Replace `wlan0` with an interface actually available on your machine.

Capture continuously until Ctrl+C:

```bash
python sniffer.py -i wlan0 -c 0 -o demo.csv
```

Enable an optional eight-byte ICMP payload preview:

```bash
python sniffer.py -i wlan0 -c 10 --hex-preview 8 -o demo.csv
```

The preview is disabled by default. Never use this tool to collect credentials or inspect traffic without authorization.

## CSV output

The stable CSV columns are:

- `timestamp`
- `src`
- `dst`
- `src_port`
- `dst_port`
- `ip_version`
- `protocols`
- `length`
- `hex_preview`

Illustrative example only:

```csv
timestamp,src,dst,src_port,dst_port,ip_version,protocols,length,hex_preview
2026-01-01T12:00:00.000+00:00,192.0.2.10,198.51.100.20,53000,53,IPv4,IPv4/UDP/DNS,75,
```

The example is fabricated for documentation and is not an actual capture.

## Testing

```bash
python -m py_compile sniffer.py tests/test_sniffer.py
python -m pytest -v
```

Offline tests use synthetic packets. Passing them does not prove live packet capture works.

## Permission troubleshooting

Packet capture may require elevated privileges. First try your normal account. If necessary, use an approved least-privilege method. For a short authorized demonstration, you can try:

```bash
sudo "$PWD/.venv/bin/python" sniffer.py -i lo -c 5 -o demo.csv
```

Use `lo` only if available and suitable for local testing. Root may create files owned by root.

## Privacy and responsible use

Only monitor networks you own or have explicit permission to assess. CSV metadata can be sensitive. Do not commit real captures or sensitive CSV files to a public repository. Do not report test or capture results until you have observed them.

## Project files

- `sniffer.py` — parsing and capture logic
- `tests/test_sniffer.py` — automated offline tests
- `requirements.txt` — dependencies
- `.gitignore` — excludes generated and sensitive files
- `README.md` — project documentation

## Verification record

Update these entries only after performing the checks:

- Python version: not yet verified for this project
- Dependencies: not yet verified
- Compilation: not yet run
- Pytest: not yet run
- Live capture: not yet run
- Git commit and push: not yet completed
