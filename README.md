# riscure-models

Pydantic models for Riscure FI/SCA probe specifications and Moku integration.

**For detailed usage and development guidelines, see [CLAUDE.md](CLAUDE.md)**

## Overview

**riscure-models** provides type-safe data models for Riscure electromagnetic fault injection (EM-FI) and side-channel analysis (SCA) probes, with first-class integration with the moku-instrument-forge ecosystem.

**Purpose**:
- Define electrical specifications for Riscure probes (ports, voltage ranges, impedances)
- Enable wiring diagram validation between Moku platforms and Riscure probes
- Ensure voltage compatibility and safety before hardware connection

**Design Goals**:
- Mirror moku-models architecture for seamless integration
- Prioritize voltage compatibility checking for safety
- Machine-readable specifications for automated validation

## Supported Probes

- **DS1120A**: High-power unidirectional EM-FI probe (450V, 64A, fixed 50ns pulse)
- **DS1121A**: Bidirectional EM-FI probe with sensing capability *(coming soon)*

## Installation

```bash
# Development mode
cd riscure-models/
uv pip install -e .
```

## Quick Start

```python
from riscure_models import DS1120APlatform
from moku_models import MOKU_GO_PLATFORM

# Load probe specification
probe = DS1120APlatform()

# Access port specifications
trigger_port = probe.get_port_by_id('digital_glitch')
print(f"Trigger input: {trigger_port.voltage_min}V to {trigger_port.voltage_max}V")

# Validate compatibility with Moku output
moku_output = MOKU_GO_PLATFORM.get_analog_output_by_id('OUT1')
# Validation logic coming soon
```

## Core Models

### Platform Models
- `DS1120APlatform`: Complete electrical specification for DS1120A probe
  - Signal ports (SMA): digital_glitch, pulse_amplitude, coil_current
  - Power port: 24-450V DC input
  - Physical probe tips: 1.5mm, 4mm, crescent variants

### Port Models
Uses moku-models `AnalogPort` for consistency:
- Port identifier (e.g., 'digital_glitch')
- Direction (input/output)
- Voltage ranges (voltage_min, voltage_max)
- Impedance specifications
- Connector types

## Project Structure

```
riscure_models/
├── __init__.py              # Public API exports
└── probes/
    ├── __init__.py
    ├── ds1120a.py          # DS1120APlatform model
    └── ds1121a.py          # (future)
```

## Integration with moku-instrument-forge

Import alongside moku-models for validation:

```python
from moku_models import MokuConfig, MOKU_GO_PLATFORM
from riscure_models import DS1120APlatform

# Define Moku deployment
moku_config = MokuConfig(platform=MOKU_GO_PLATFORM, ...)

# Define probe specification
probe = DS1120APlatform()

# Validate wiring compatibility (future)
# validate_connection(moku_config, probe)
```

## Development Status

- [x] Initial repository setup
- [x] DS1120A port specifications
- [x] DS1120A platform model
- [x] Voltage compatibility validation (port-level)
- [ ] Cross-platform validation with moku-models
- [ ] DS1121A support
- [ ] Automated wiring diagram generation

## References

- **Datasheet**: DS1120A/DS1121A official documentation
- **Parent Project**: [moku-instrument-forge](https://github.com/sealablab/moku-instrument-forge)
- **Related Library**: [moku-models](https://github.com/sealablab/moku-models)

## License

MIT
