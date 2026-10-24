#!/usr/bin/env -S uv run --script
#
# /// script
# requires-python = ">=3.14.7"
# dependencies = ["evdev"]
# ///


from evdev import list_devices, InputDevice, categorize, ecodes


HEADPHONES_MAC = "94:53:30:18:b5:b8"


def find_headphones() -> InputDevice | None:
    for path in list_devices():
        dev = InputDevice(path)
        mac = dev.phys
        if mac == HEADPHONES_MAC:
            return dev
        dev.close()
    return None

def main():
    dev = find_headphones()
    if dev is None:
        raise SystemExit(f"Could not find the headphones with MAC {HEADPHONES_MAC}")

    print(f"Headphones {dev.name} found, grabbing all events from the device")
    dev.grab()
    try:
        for event in dev.read_loop():
            if event.type == ecodes.EV_KEY:
                print(f"Grabbed: {categorize(event)}")
    except KeyboardInterrupt:
        pass
    except OSError as e:
        print(f"Device error (disconnected?): {e}")
    finally:
        try:
            dev.ungrab()
        except OSError:
            pass 
        dev.close()
        print(f"Headphones {dev.name} released")

    

if __name__ == "__main__":
    main()