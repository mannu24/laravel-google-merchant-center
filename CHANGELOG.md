# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2024-12-19

### Fixed
- **Critical**: Fixed namespace inconsistency across all source files
- Updated all classes to use `Mannu24\GoogleMerchantCenter` namespace
- Renamed service provider from `GMCServiceProvider` to `GoogleMerchantCenterServiceProvider`
- Fixed autoloading issues and PSR-4 compliance
- Updated all import statements and class references

## [1.0.0] - 2024-12-19

### Added
- Initial release of Laravel Google Merchant Center integration package
- GMCProduct model for managing Google Merchant Center products
- GMCSyncLog model for tracking synchronization operations
- GMCService for handling Google API interactions
- GMCRepository for data persistence operations
- SyncsWithGMC trait for easy model integration
- SyncAllProductsCommand for console-based synchronization
- GoogleMerchantCenterServiceProvider for Laravel service registration
- Comprehensive configuration file (config/gmc.php)
- Database migrations for GMC products, fields, and sync logs
- Unit tests with PHPUnit
- Usage examples and documentation

### Features
- Product synchronization with Google Merchant Center
- Batch processing capabilities
- Automatic sync management
- Laravel 8.x to 12.x compatibility
- PHP 8.0+ support
- Google API Client integration
- Console commands for automation
- Comprehensive error logging and tracking

### Technical Details
- PSR-4 autoloading
- Laravel service provider integration
- Database migration support
- Test coverage with Orchestra Testbench
- MIT License
