# Gems

## Create a gem
```sh
bundle gem name_of_the_gem

# Example
bundle gem excel_merge
cd excel_merge
```

## Annotate Gem
```sh
# Annotate only models excluding others
annotate --exclude tests,fixtures,factories,serializers --position after --show-indexes
```