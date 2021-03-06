# Package refgenconf Documentation

## Class GenomeConfigFormatError
Exception for invalid genome config file format.


## Class MissingAssetError
Error type for request of an unavailable genome asset.


## Class UnboundEnvironmentVariablesError
Use of environment variable that isn't bound to a value.


## Class RefgenconfError
Base exception type for this package


## Class MissingGenomeError
Error type for request of unknown genome/assembly.


## Class RefGenConf
A sort of oracle of available reference genome assembly assets


### assets\_dict
Map each assembly name to a list of available asset names.
```python
def assets_dict(self, order=None)
```

#### Parameters:

- `order` -- ``:  function(str) -> object how to key genome IDs for sort


#### Returns:

`Mapping[str, Iterable[str]]`:  mapping from assembly name tocollection of available asset names.




### assets\_str
Create a block of text representing genome-to-asset mapping.
```python
def assets_str(self, offset_text='  ', asset_sep='; ', genome_assets_delim=': ', order=None)
```

#### Parameters:

- `offset_text` -- `str`:  text that begins each line of the textrepresentation that's produced
- `asset_sep` -- `str`:  the delimiter between names of types of assets,within each genome line
- `genome_assets_delim` -- `str`:  the delimiter to place betweenreference genome assembly name and its list of asset names
- `order` -- ``:  function(str) -> object how to key genome IDs and assetnames for sort


#### Returns:

`str`:  text representing genome-to-asset mapping




### filepath
Determine path to a particular asset for a particular genome.
```python
def filepath(self, genome, asset, ext='.tar')
```

#### Parameters:

- `genome` -- `str`:  reference genome iD
- `asset` -- `str`:  asset name
- `ext` -- `str`:  file extension


#### Returns:

`str`:  path to asset for given genome and asset kind/name




### genomes\_list
Get a list of this configuration's reference genome assembly IDs.
```python
def genomes_list(self, order=None)
```

#### Returns:

`Iterable[str]`:  list of this configuration's reference genomeassembly IDs




### genomes\_str
Get as single string this configuration's reference genome assembly IDs.
```python
def genomes_str(self, order=None)
```

#### Parameters:

- `order` -- ``:  function(str) -> object how to key genome IDs for sort


#### Returns:

`str`:  single string that lists this configuration's knownreference genome assembly IDs




### get\_asset
Get an asset for a particular assembly.
```python
def get_asset(self, genome_name, asset_name, strict_exists=True, check_exist=<function RefGenConf.<lambda> at 0x7f56390c3378>)
```

#### Parameters:

- `genome_name` -- `str`:  name of a reference genome assembly of interest
- `asset_name` -- `str`:  name of the particular asset to fetch
- `strict_exists` -- `bool | NoneType`:  how to handle case in whichpath doesn't exist; True to raise IOError, False to raise RuntimeWarning, and None to do nothing at all
- `check_exist` -- `function(callable) -> bool`:  how to check forasset/path existence


#### Returns:

`str`:  path to the asset


#### Raises:

- `TypeError`:  if the existence check is not a one-arg function
- `refgenconf.MissingGenomeError`:  if the named assembly isn't knownto this configuration instance
- `refgenconf.MissingAssetError`:  if the names assembly is known tothis configuration instance, but the requested asset is unknown




### list\_assets\_by\_genome
List types/names of assets that are available for one--or all--genomes.
```python
def list_assets_by_genome(self, genome=None, order=None)
```

#### Parameters:

- `genome` -- `str | NoneType`:  reference genome assembly ID, optional;if omitted, the full mapping from genome to asset names
- `order` -- ``:  function(str) -> object how to key genome IDs and assetnames for sort


#### Returns:

`Iterable[str] | Mapping[str, Iterable[str]]`:  collection ofasset type names available for particular reference assembly if one is provided, else the full mapping between assembly ID and collection available asset type names




### list\_genomes\_by\_asset
List assemblies for which a particular asset is available.
```python
def list_genomes_by_asset(self, asset=None, order=None)
```

#### Parameters:

- `asset` -- `str | NoneType`:  name of type of asset of interest, optional
- `order` -- ``:  function(str) -> object how to key genome IDs and assetnames for sort


#### Returns:

`Iterable[str] | Mapping[str, Iterable[str]]`:  collection ofassemblies for which the given asset is available; if asset argument is omitted, the full mapping from name of asset type to collection of assembly names for which the asset key is available will be returned.




### list\_local
List locally available reference genome IDs and assets by ID.
```python
def list_local(self, order=None)
```

#### Parameters:

- `order` -- ``:  function(str) -> object how to key genome IDs and assetnames for sort


#### Returns:

`str, str`:  text reps of locally available genomes and assets




### list\_remote
List genomes and assets available remotely.
```python
def list_remote(self, get_url=<function RefGenConf.<lambda> at 0x7f56390c3620>, order=None)
```

#### Parameters:

- `get_url` -- `function(refgenconf.RefGenConf) -> str`:  how to determineURL request, given RefGenConf instance
- `order` -- ``:  function(str) -> object how to key genome IDs and assetnames for sort


#### Returns:

`str, str`:  text reps of remotely available genomes and assets




### pull\_asset
Download and possibly unpack one or more assets for a given ref gen.
```python
def pull_asset(self, genome, assets, genome_config, unpack=True, force=None, get_json_url=<function RefGenConf.<lambda> at 0x7f56390c3730>, get_main_url=None, build_signal_handler=<function _handle_sigint at 0x7f56395e78c8>)
```

#### Parameters:

- `genome` -- `str`:  name of a reference genome assembly of interest
- `assets` -- `str`:  name(s) of particular asset(s) to fetch
- `genome_config` -- `str`:  path to genome configuration file to update
- `unpack` -- `bool`:  whether to unpack a tarball
- `force` -- `bool | NoneType`:  how to handle case in which asset pathalready exists; null for prompt (on a per-asset basis), False to effectively auto-reply No to the prompt to replace existing file, and True to auto-replay Yes for existing asset replacement.
- `get_json_url` -- `function(str, str, str) -> str`:  how to build URL fromgenome server URL base, genome, and asset
- `get_main_url` -- `function(str) -> str`:  how to get archive URL frommain URL
- `build_signal_handler` -- `function(str) -> function`:  how to createa signal handler to use during the download; the single argument to this function factory is the download filepath


#### Returns:

`Iterable[(str, str | NoneType)]`:  collection of pairs of assetname and folder name (key-value pair with which genome config file is updated) if pull succeeds, else asset key and a null value.


#### Raises:

- `TypeError`:  if the assets argument is neither string nor otherIterable
- `refgenconf.UnboundEnvironmentVariablesError`:  if genome folderpath contains any env. var. that's unbound




### update\_genomes
Updates the genomes in RefGenConf object at any level. If a requested genome-asset mapping is missing, it will be created
```python
def update_genomes(self, genome, asset=None, data=None)
```

#### Parameters:

- `genome` -- `str`:  genome to be added/updated
- `asset` -- `str`:  asset to be added/updated
- `data` -- `Mapping`:  data to be added/updated


#### Returns:

`RefGenConf`:  updated object




## Class MissingConfigDataError
Missing required configuration instance items


### select\_genome\_config
Get path to genome configuration file.
```python
def select_genome_config(filename, conf_env_vars=None, **kwargs)
```

#### Parameters:

- `filename` -- `str`:  name/path of genome configuration file
- `conf_env_vars` -- `Iterable[str]`:  names of environment variables toconsider; basically, a prioritized search list


#### Returns:

`str`:  path to genome configuration file





**Version Information**: `refgenconf` v0.2.1-dev, generated by `lucidoc` v0.4dev