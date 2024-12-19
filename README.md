# Qvistens - Asset Resolver - PythonExpose
**Author:** Raymond Kreppene

This repository houses the PythonExpose, 
a Python script used as a component in Luca's cache resolver. 
For more information on the cache resolver, visit [Luca Scheller's VFX-UsdAssetResolver on GitHub](https://github.com/LucaScheller/VFX-UsdAssetResolver).

## Features

- **Resolve Latest**: Replaces the keyword 'latest' with the latest version on disk.
- **Resolve Anchor**: Replaces the anchor in your path with the highest priority anchor where the file actually exists.

## Setup Environment Variables
To make the asset resolver function properly, set the following environment variables:

- `PXR_PLUGINPATH_NAME`: Path to the folder containing `plugInfo.json`. 
- `PYTHONPATH`: Path where the `PythonExpose.py` script is located. 
- `AR_EXPOSE_ABSOLUTE_PATH_IDENTIFIERS`: Set to `1` to enable evaluation of absolute paths by the resolver.
- `AR_CACHE_ASSETPATHS`: Enable or diable path caching. 
- `AR_SEARCH_PATHS`:  This is variable contains the different searchpath that replaces the anchor of the path. Linux and windows path sepearated by ?

**Example:**

- **PXR_PLUGINPATH_NAME** $USD_ASSET_RESOLVER/cachedResolver/resources
- **PYTHONPATH** $USD_ASSET_RESOLVER/cachedResolver/lib/python
- **AR_EXPOSE_ABSOLUTE_PATH_IDENTIFIERS** 1
- **AR_CACHE_ASSETPATHS** 1
- **AR_SEARCH_PATHS**  \/cache\/\$\{PROJECTUNC\}:\$\{PROJECTUNC\}:/cache/\$\{RENDERTEMP\}:\$\{TEMPSTORAGE\}?\$\{PROJECTDIR\};\$\{RENDERTEMP\};\$\{TEMPSTORAGE\}


## Debug Environment Variables

- `PRINT_RESOLVED_PATH`: If set to `True`, prints resolved paths for debugging purposes.
- `TF_DEBUG`: Enables debugging output for asset resolution. TF_DEBUG=AR_RESOLVER_INIT


## Usage
Once you made sure all env vars is setup, the asset resolver should be working in your USD applications.
you can confirm this in many ways. 

One way to confirm this is to set TF_DEBUG env var
Setting the environment variable to TF_DEBUG will print somthing like this to your consol, when starting you applicatioon

        ArGetResolver(): Found primary asset resolver types: [CachedResolver, ArDefaultResolver]
        ArGetResolver(): Using asset resolver CachedResolver from plugin  ..../assetResolver/cachedResolver/lib/cachedResolver.dll for primary resolver
        ArGetResolver(): Found URI resolver ArDefaultResolver
        ArGetResolver(): Found URI resolver CachedResolver
        ArGetResolver(): Found URI resolver FS_ArResolver
        ArGetResolver(): Using FS_ArResolver for URI scheme(s) ["op", "opdef", "oplib", "opdatablock"]
        ArGetResolver(): Found package resolver Usd_UsdzResolver
        ArGetResolver(): Using package resolver Usd_UsdzResolver for usdz from plugin usd
        ArGetResolver(): Found package resolver USD_NcPackageResolver
        ArGetResolver(): Using package resolver USD_NcPackageResolver for usdlc from plugin usdNc
        ArGetResolver(): Using package resolver USD_NcPackageResolver for usdnc from plugin usdNc

To confirm your paths is resolved as you expect you can set the PRINT_RESOLVED_PATH to True.
this will print somthing like this

        remove_anchor_pre: - M:/film/feature/asset/prop/bookD/bookD.usda
        remove_anchor_post: - asset/prop/bookD/bookD.usda
        newResolvedPath: - \\unc.path.com\film\feature\asset\prop\bookD\bookD.usda

        remove_anchor_pre: - //unc.path.com/film/feature/asset/prop/bookD/look/default/pub/latest/bookD-look-default-latest.usd
        remove_anchor_post: - asset/prop/bookD/look/default/pub/latest/bookD-look-default-latest.usd
        newResolvedPath: - \\unc.path.com\film\feature\asset\prop\bookD\look\default\pub\v002\bookD-look-default-v002.usd

        remove_anchor_pre: - //unc.path.com/film/feature/asset/prop/bookD/model/default/pub/latest/bookD-model-default-latest.usd
        remove_anchor_post: - asset/prop/bookD/model/default/pub/latest/bookD-model-default-latest.usd
        newResolvedPath: - \\unc.path.com\film\feature\asset\prop\bookD\model\default\pub\v003\bookD-model-default-v003.usd


## Dev
if you need to make any changes to PythonExpose, it handy to reload the script from inside your host appplication when making changes, this way you dont have to constantly restart your application to test the changes 

```python
import sys
import PythonExpose
from importlib import reload  # Python 3.4+

sys.path.append('k:/path/to/assetresolver/cachedResolver/lib/python')

reload(PythonExpose)

```

## License

This script is provided as-is without any warranty. Use at your own risk.