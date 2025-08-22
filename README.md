# Laravel Google Merchant Center Integration

A comprehensive Laravel package for seamless Google Merchant Center product synchronization with independent tables, batch processing, and automatic sync management.

## Features

- 🚀 **Product Synchronization** - Sync products with Google Merchant Center
- 📦 **Batch Processing** - Handle large datasets efficiently
- 🔄 **Automatic Sync** - Set up automated synchronization workflows
- 🎯 **Per-Product Control** - Enable/disable sync for individual products
- 📊 **Sync Logging** - Comprehensive tracking and monitoring
- 🛡️ **Error Handling** - Robust retry logic and error management
- ⚡ **Performance Optimized** - Rate limiting and caching
- 🔧 **Laravel Integration** - Seamless Laravel ecosystem integration

## Requirements

- PHP 8.0 or higher
- Laravel 8.x, 9.x, 10.x, 11.x, or 12.x
- Google Merchant Center account
- Google API credentials

## Installation

```bash
composer require mannu24/google-merchant-center
```

## Quick Setup

1. **Publish configuration:**
```bash
php artisan vendor:publish --tag=gmc-config
```

2. **Publish migrations:**
```bash
php artisan vendor:publish --tag=gmc-migrations
```

3. **Run migrations:**
```bash
php artisan migrate
```

4. **Configure environment variables:**
```env
GMC_MERCHANT_ID=your_merchant_id
GMC_SERVICE_ACCOUNT_JSON=/path/to/service-account.json
GMC_AUTO_SYNC=true
GMC_BATCH_SIZE=50
GMC_RETRY_ATTEMPTS=3
```

## Basic Usage

### 1. Add Trait to Your Product Model

```php
use Mannu24\GoogleMerchantCenter\Traits\SyncsWithGMC;

class Product extends Model
{
    use SyncsWithGMC;
    
    protected $fillable = [
        'title', 'description', 'price', 'quantity', 
        'image_url', 'brand', 'sku', 'status'
    ];
    
    public function prepareGMCData(): array
    {
        return [
            'offerId' => $this->sku ?: (string) $this->id,
            'title' => $this->title,
            'description' => $this->description,
            'link' => url("/products/{$this->id}"),
            'imageLink' => $this->image_url,
            'price' => ['value' => (string) $this->price, 'currency' => 'USD'],
            'availability' => $this->quantity > 0 ? 'in stock' : 'out of stock',
            'brand' => $this->brand,
            'condition' => 'new'
        ];
    }
}
```

### 2. Sync Products

```php
// Manual sync
$product->syncToGMC();

// Force sync (ignores cache)
$product->forceSyncToGMC();

// Check sync status
if ($product->isSyncedWithGMC()) {
    echo "Product is synced!";
}
```

### 3. Bulk Operations

```php
use Mannu24\GoogleMerchantCenter\Services\GMCService;

$gmcService = app(GMCService::class);
$result = $gmcService->syncMultipleProducts($products, 25);
```

### 4. Artisan Commands

```bash
# Sync all products
php artisan gmc:sync-all

# Sync with filters
php artisan gmc:sync-all --filter="status=active"

# Dry run (no actual sync)
php artisan gmc:sync-all --dry-run

# Force sync (ignore cache)
php artisan gmc:sync-all --force
```

## Database Structure

### `gmc_products` Table
- `product_id` - Reference to your product
- `product_type` - Model class name
- `sync_enabled` - Control sync per product
- `gmc_product_id` - GMC's product ID
- `gmc_last_sync` - Last sync timestamp
- `sync_status` - Current sync status
- `last_error` - Last error message

### `gmc_sync_logs` Table
- Tracks all sync attempts
- Stores request/response data
- Performance metrics
- Error details

## Advanced Features

### Batch Processing
```php
$gmcService = app(GMCService::class);
$gmcService->setBatchSize(25);
$result = $gmcService->syncMultipleProducts($products);
```

### Error Handling
```php
try {
    $product->syncToGMC();
} catch (\Exception $e) {
    Log::error('GMC Sync failed: ' . $e->getMessage());
}
```

### Sync History
```php
use Mannu24\GoogleMerchantCenter\Models\GMCSyncLog;

$logs = GMCSyncLog::with('gmcProduct')->latest()->get();
$failedLogs = GMCSyncLog::where('status', 'failed')->get();
```

## Configuration

The package configuration file (`config/gmc.php`) includes:

- API credentials and endpoints
- Batch processing settings
- Retry and timeout configurations
- Logging preferences
- Cache settings

## Testing

```bash
composer test
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This package is open-sourced software licensed under the [MIT license](LICENSE).

## Support

- [GitHub Issues](https://github.com/mannu24/laravel-google-merchant-center/issues)
- [Documentation](https://github.com/mannu24/laravel-google-merchant-center) 