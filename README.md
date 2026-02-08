# py_canoe

[![PyPI version](https://badge.fury.io/py/py-canoe.svg)](https://pypi.org/project/py_canoe/)
[![Python versions](https://img.shields.io/pypi/pyversions/py_canoe.svg)](https://pypi.org/project/py_canoe/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**py_canoe** is a Python package for programmatically controlling Vector CANoe, enabling automation of CAN network testing, simulation, and diagnostics.

## Features

- 🚀 **Full CANoe Automation** - Control CANoe configurations, measurements, and operations
- 📊 **Signal Access** - Read and write CAN signals with raw and physical values
- 🔧 **Diagnostics Support** - Send diagnostic requests and control tester present
- 📝 **System Variables** - Get, set, and define system variables
- 🎯 **Test Execution** - Execute test modules and test environments
- 📄 **Offline Mode** - Work with offline configurations and replay blocks
- 🔍 **Bus Statistics** - Monitor CAN bus statistics in real-time
- 💻 **CAPL Integration** - Compile CAPL nodes and call CAPL functions

## Quick Links

- 📖 [Documentation](https://chaitu-ycr.github.io/py_canoe/)
- 📦 [PyPI Package](https://pypi.org/project/py_canoe/)
- 🎉 [Releases](https://github.com/chaitu-ycr/py_canoe/releases)
- 💡 [Discussions](https://github.com/chaitu-ycr/py_canoe/discussions) - Ideas and suggestions
- 🐛 [Issues](https://github.com/chaitu-ycr/py_canoe/issues/new/choose) - Bug reports and feature requests

## Prerequisites

- **Python 3.9 or higher** - [Download Python](https://www.python.org/downloads/)
- **Vector CANoe v11 or higher** - [Download CANoe](https://www.vector.com/int/en/support-downloads/download-center/)
- **Windows Operating System** - Windows 10 or later recommended (16GB RAM recommended)
- **Code Editor** - [Visual Studio Code](https://code.visualstudio.com/Download) or your preferred IDE

## Installation

### Create a Python Virtual Environment

```bat
python -m venv .venv
```

### Activate the Virtual Environment

```bat
.venv\Scripts\activate
```

### Install py_canoe

```bat
pip install py_canoe --upgrade
```

### Optional: Upgrade pip

```bat
python -m pip install pip --upgrade
```

## Quick Start

```python
from py_canoe import CANoe

# Create CANoe instance
canoe_inst = CANoe()

# Open configuration and start measurement
canoe_inst.open(canoe_cfg=r'path\to\config.cfg')
canoe_inst.start_measurement()

# Get CANoe version information
version_info = canoe_inst.get_canoe_version_info()
print(f"CANoe Version: {version_info}")

# Stop measurement and close
canoe_inst.stop_measurement()
canoe_inst.quit()
```

## Usage Examples

### Import CANoe Module and Create Instance

```python
from py_canoe import CANoe
from time import sleep as wait

canoe_inst = CANoe()
```

### Open CANoe Configuration and Control Measurement

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo.cfg')
canoe_inst.start_measurement()
canoe_version_info = canoe_inst.get_canoe_version_info()
canoe_inst.stop_measurement()
canoe_inst.quit()
```

### Restart/Reset Running Measurement

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo.cfg')
canoe_inst.start_measurement()
canoe_inst.reset_measurement()
canoe_inst.stop_ex_measurement()
```

### Work with Offline Configurations

```python
canoe_inst.open(r'tests\demo_cfg\demo_offline.cfg')
canoe_inst.add_offline_source_log_file(r'tests\demo_cfg\Logs\demo_log.blf')
canoe_inst.start_measurement_in_animation_mode(animation_delay=200)
wait(1)
canoe_inst.break_measurement_in_offline_mode()
wait(1)
canoe_inst.step_measurement_event_in_single_step()
wait(1)
canoe_inst.reset_measurement_in_offline_mode()
wait(1)
canoe_inst.stop_measurement()
wait(1)
```

### Get/Set CANoe Measurement Index

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
meas_index_value = canoe_inst.get_measurement_index()
canoe_inst.start_measurement()
canoe_inst.stop_measurement()
meas_index_value = canoe_inst.get_measurement_index()
canoe_inst.set_measurement_index(meas_index_value + 1)
meas_index_new = canoe_inst.get_measurement_index()
canoe_inst.reset_measurement()
canoe_inst.stop_measurement()
```

### Save CANoe Configuration to Different Version

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.save_configuration_as(path=r'tests\demo_cfg\demo_v10.cfg', major=10, minor=0, create_dir=True)
```

### Get CAN Bus Statistics

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.start_measurement()
wait(2)
canoe_inst.get_can_bus_statistics(channel=1)
canoe_inst.stop_measurement()
```

### Work with Bus Signals

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.start_measurement()
wait(1)
sig_full_name = canoe_inst.get_signal_full_name(bus='CAN', channel=1, message='LightState', signal='FlashLight')
sig_value = canoe_inst.get_signal_value(bus='CAN', channel=1, message='LightState', signal='FlashLight', raw_value=False)
canoe_inst.set_signal_value(bus='CAN', channel=1, message='LightState', signal='FlashLight', value=1, raw_value=False)
wait(1)
sig_online_state = canoe_inst.check_signal_online(bus='CAN', channel=1, message='LightState', signal='FlashLight')
sig_state = canoe_inst.check_signal_state(bus='CAN', channel=1, message='LightState', signal='FlashLight')
sig_val = canoe_inst.get_signal_value(bus='CAN', channel=1, message='LightState', signal='FlashLight', raw_value=True)
canoe_inst.stop_measurement()
```

### Control Write Window

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.enable_write_window_output_file(r'tests\demo_cfg\Logs\write_win.txt')
wait(1)
canoe_inst.start_measurement()
canoe_inst.clear_write_window_content()
wait(1)
canoe_inst.write_text_in_write_window("hello from py_canoe!")
wait(1)
text = canoe_inst.read_text_from_write_window()
canoe_inst.stop_measurement()
canoe_inst.disable_write_window_output_file()
wait(1)
```

### Switch Between CANoe Desktops

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.ui_activate_desktop('Configuration')
```

### Work with System Variables

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.start_measurement()
wait(1)
canoe_inst.set_system_variable_value('demo::level_two_1::sys_var2', 20)
canoe_inst.set_system_variable_value('demo::string_var', 'hey hello this is string variable')
canoe_inst.set_system_variable_value('demo::data_var', 'hey hello this is data variable')
canoe_inst.set_system_variable_array_values('demo::int_array_var', (00, 11, 22, 33, 44, 55, 66, 77, 88, 99))
wait(0.1)
sys_var_val = canoe_inst.get_system_variable_value('demo::level_two_1::sys_var2')
sys_var_val = canoe_inst.get_system_variable_value('demo::data_var')
canoe_inst.stop_measurement()
# define system variable and use it in measurement
canoe_inst.define_system_variable('sys_demo::demo', 1)
canoe_inst.save_configuration()
canoe_inst.start_measurement()
wait(1)
sys_var_val = canoe_inst.get_system_variable_value('sys_demo::demo')
canoe_inst.stop_measurement()
```

### Send Diagnostic Requests

```python
canoe_inst.open(r'tests\demo_cfg\demo_diag.cfg')
canoe_inst.start_measurement()
wait(1)
resp = canoe_inst.send_diag_request('Door', 'DefaultSession_Start', False)
canoe_inst.control_tester_present('Door', False)
wait(2)
canoe_inst.control_tester_present('Door', True)
wait(5)
resp = canoe_inst.send_diag_request('Door', '10 02')
canoe_inst.control_tester_present('Door', False)
wait(2)
resp = canoe_inst.send_diag_request('Door', '10 03', return_sender_name=True)
canoe_inst.stop_measurement()
```

### Control Replay Blocks

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.start_measurement()
wait(1)
canoe_inst.set_replay_block_file(block_name='DemoReplayBlock', recording_file_path=r'tests\demo_cfg\Logs\demo_log.blf')
wait(1)
canoe_inst.control_replay_block(block_name='DemoReplayBlock', start_stop=True)
wait(2)
canoe_inst.control_replay_block(block_name='DemoReplayBlock', start_stop=False)
wait(1)
canoe_inst.stop_measurement()
```

### Compile CAPL and Call Functions

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.compile_all_capl_nodes()
canoe_inst.start_measurement()
wait(1)
canoe_inst.call_capl_function('addition_function', 100, 200)
canoe_inst.call_capl_function('hello_world')
canoe_inst.stop_measurement()
```

### Execute Test Modules

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.start_measurement()
wait(1)
canoe_inst.execute_all_test_modules_in_test_env(demo_test_environment)
canoe_inst.execute_test_module('demo_test_node_002')
wait(1)
canoe_inst.stop_measurement()
```

### Work with Environment Variables

```python
canoe_inst.open(canoe_cfg=r'tests\demo_cfg\demo_dev.cfg')
canoe_inst.start_measurement()
wait(1)
canoe_inst.set_environment_variable_value('int_var', 123.12)
canoe_inst.set_environment_variable_value('float_var', 111.123)
canoe_inst.set_environment_variable_value('string_var', 'this is string variable')
canoe_inst.set_environment_variable_value('data_var', (1, 2, 3, 4, 5, 6, 7))
var_value = canoe_inst.get_environment_variable_value('int_var')
var_value = canoe_inst.get_environment_variable_value('float_var')
var_value = canoe_inst.get_environment_variable_value('string_var')
var_value = canoe_inst.get_environment_variable_value('data_var')
wait(1)
canoe_inst.stop_measurement()
```

## Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository** - Create your own fork of [py_canoe](https://github.com/chaitu-ycr/py_canoe/fork)
2. **Create a feature branch** - `git checkout -b feature/amazing-feature`
3. **Make your changes** - Follow the existing code style
4. **Test your changes** - Ensure everything works as expected
5. **Commit your changes** - `git commit -m 'Add amazing feature'`
6. **Push to the branch** - `git push origin feature/amazing-feature`
7. **Open a Pull Request** - Submit your PR for review

Please read our [Code of Conduct](CODE_OF_CONDUCT.md) before contributing.

## Support

- 📖 **Documentation**: [https://chaitu-ycr.github.io/py_canoe/](https://chaitu-ycr.github.io/py_canoe/)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/chaitu-ycr/py_canoe/discussions) for questions and ideas
- 🐛 **Bug Reports**: [GitHub Issues](https://github.com/chaitu-ycr/py_canoe/issues/new/choose)
- ✨ **Feature Requests**: [GitHub Issues](https://github.com/chaitu-ycr/py_canoe/issues/new/choose)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

**chaitu-ycr** - [chaitu.ycr@gmail.com](mailto:chaitu.ycr@gmail.com)

## Acknowledgments

- Vector Informatik GmbH for the CANoe software
- The Python community for the excellent pywin32 package
