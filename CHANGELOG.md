# Changelog

All notable changes to this project will be documented in this file.

## [2.0.1] - 2025-10-20

### Changed
- Removed legacy tapo-s220 node (use tapo-switch instead)
- Cleaned up all logging (errors only, removed debug/info spam)
- Clean git history for professional appearance

### Fixed
- Package metadata improvements

## [2.0.0] - 2025-10-19

### Added
- Hub config node architecture for shared credentials
- T310/T315 temperature and humidity sensor support
- Request queue to prevent concurrent request conflicts
- Battery and signal strength monitoring
- Clean example flows
- Comprehensive documentation

### Changed
- **BREAKING:** Package renamed from `node-red-contrib-tapo-s220` to `node-red-contrib-tapo-hub`
- **BREAKING:** Removed legacy `tapo-s220` node
- Reduced logging (errors only, no debug/info spam)
- Improved node status displays
- Updated README for npm publication

### Fixed
- Concurrent request handling (request queue implementation)
- Node status indicator accuracy
- Error handling improvements

### Supported Devices
- S220/S210 smart switches
- T310/T315 temperature/humidity sensors

## [1.0.0] - 2025-10-17

### Added
- Initial release
- Basic S220 switch control via H100 hub
- Device discovery
- Switch on/off/toggle commands
- Status queries

### Known Issues
- Concurrent requests could cause stuck/no-data responses (fixed in 2.0.0)

