# candb-bridge

A small Python process that bridges [python-can](https://python-can.readthedocs.io/)
to the **candb-studio** VS Code extension over newline-delimited **JSON-RPC 2.0**
on stdio. It lets the extension talk to vendor CAN hardware (PEAK/PCAN, Kvaser,
Vector, SocketCAN) without shipping native addons in the extension host.

See `specs/007-multi-can-adapters/architecture.md` in the extension repo for the
full protocol.

## Install

This package is not published to PyPI. Install it from a local checkout
of the CANdb Studio reposity with the following command, executed from a 
command shell at the root of the CANdb Studio checkout folder:

```bash
pip install -e ./bridge
```

Ensure that after installation of the bridge, the installation target location
reported by pip is included in the %PATH% environment variable.

The extension launches the bridge as `python -m candb_bridge`. Override the
interpreter with the `CANDB_BRIDGE_PYTHON` environment variable if needed
(e.g. set it to `py` or an absolute path).

## Protocol summary

- On start the bridge emits a `ready` notification with `{version, capabilities}`.
- The extension replies with `ack`.
- Requests: `enumerate`, `connect`, `send`, `disconnect`.
- Notifications: `frame`, `state`, `ready`, `error`.
