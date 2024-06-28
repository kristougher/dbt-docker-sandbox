# DBT Playground

Docker based DBT Bigquery playground based on the [DBT core install guide](https://docs.getdbt.com/guides/manual-install). DBT Cloud was deliberately avoided to postpone vendor lock-in.

With this, you do not need to install DBT locally and you can jump directly to [step 8 of the tutorial](https://docs.getdbt.com/guides/manual-install?step=8).

The Jaffle shop name is used in this project as well to make it easier to use with the tutorial.

## Generally
DBT is basically a template based data conversion tool that handles a lot of the boilerplate tasks otherwise involved. The structure is very version control friendly, allowing to keep track of changes in the whole conversion chain. They put a lot more words to it themselves, but those are the basics.

The folder structure is pretty much 1:1 of the tutorial setup. An important difference is the `.settings` folder. In a 'normal' DBT install, these files would be located outside the project in your home folder.

This setup is oriented towards BigQuery. To use other databases, replace the image (or create a new service) in `docker-compose.yml` and add a profile for it in `.settings/profile.yml`.

## Setting it up
In order to start working:
* copy your GCP keyfile into the `.settings` directory. The service account must have the roles *BigQuery Job User* and *BigQuery Data Editor*
* open `.settings/profiles-example.yml`, change the name of the keyfile, project and dataset (DBT will create the latter, the project has to be there) and save it as profiles.yml in the same directory.
* Edit the SQL files in /models or create new ones
* run `docker compose run dbt-bq run` in your terminal
* Whammo! Your models are being built.

It is strongly recommended to follow the tutorial as it touches upon essential functionality.

## Running it
Having DBT running in docker, obviously requires that you execute it through Docker. The command is:
```bash
$  docker compose run dbt-bq
```
This corresponds to `dbt`. You may want to alias the above.

To run all your models, use the `run` command:
```bash
$  docker compose run dbt-bq run
```
There are a number of options and flags you can add to it. One of the more convenient is `--select` that lets you specify what to run: https://docs.getdbt.com/reference/node-selection/syntax

## Make it your own
The naming and existing models are all from the tutorial mentioned above. To create your own project, follow these steps:
* Delete all sql files and schema.yml in the models folder
* Create a keyfile for your project and place it in `.settings/`
* Update `.settings/profiles.yml` with a profile for your project and delete the Jaffle shop.
* Update `dbt_project.yml` by replacing *jaffle_shop*  with the name of your project.
And off you go.

### Resources:
- [Syntax reference](https://docs.getdbt.com/reference/node-selection/syntax)
- Learn more about dbt [in the docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Check out [the blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices

## DBT + Airflow
