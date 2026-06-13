# Data Product Template

This repository is the **generic skeleton** for every data product in the mesh.
It is a [GitHub template repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository)
— new product repos are spawned from it via the *Mesh Console* (or the GitHub
"Generate from template" button).

## Repository structure

```
product.yml                  # data-product manifest (fill in after spawning)
ports/
  input_ports.yml            # upstream data sources consumed by this product
  output_ports.yml           # output ports: S3/Parquet, Snowflake, REST API, …
config/
  ingestion_config.yml       # dlt / connector ingestion settings
  serving_config.yml         # serving-layer adapter settings
transformation/
  dbt_project.yml            # dbt project config
  profiles.yml               # dbt connection profiles
  macros/
    generate_schema_name.sql # custom schema macro (prevents env collisions)
  models/
    staging/sources.yml      # dbt source declarations
    intermediate/            # ephemeral intermediate models
    marts/schema.yml         # mart model documentation
.github/workflows/ci.yml     # CI: validate → test → deploy
.gitignore
```

## After spawning

1. Edit `product.yml` — set `name`, `domain`, `owner`, `tier`, `description`.
2. Edit `ports/input_ports.yml` — declare your upstream sources.
3. Edit `ports/output_ports.yml` — declare which output ports are active.
4. Edit `config/ingestion_config.yml` — configure your source connector.
5. Add dbt models under `transformation/models/`.
6. Push a commit — CI runs the stub pipeline.

## Local development

```bash
uv sync
uv run dbt debug --profiles-dir transformation
uv run dbt run   --profiles-dir transformation
```
