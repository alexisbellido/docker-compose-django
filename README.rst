Docker Compose for Django
===================================================================

- A Python service that includes Django and the Django REST Framework package.
- A database service running PostgreSQL.

These are the current versions:

- Django 3.1.2
- PostgreSQL 10.12
- Django REST Framework 3.12.1
- Requests 2.24.0

The project uses a `multi-stage build <https://docs.docker.com/develop/develop-images/multistage-build/>`_ for the app service, where Django runs, so you can pass a private private SSH key if you need to include private repositories. The final Docker image won't include your SSH private key.

Installation
---------------------------------------------------------------

Create a directory for static files on your host and grant it permissions so Docker can write to it.

.. code-block:: bash

    $ mkdir -p ~/project-compose-dev/static
    $ sudo chmod -R 777 ~/project-compose-dev/static

Create a copy of the provided ``config.sample.yaml``, name it ``config.yaml`` and replace the values with your project's settings.

.. code-block:: bash

    $ cp project-compose/secrets/config.sample.yaml project-compose/secrets/config.yaml

Change to the directory containing the Docker Compose YAML files and start the project.

.. code-block:: bash

    $ cd project-compose/
    $ SSH_PRIVATE_KEY="$(cat ~/.ssh/id_rsa)" docker-compose up

Log in to the app container to initialize Django's database, create a super user and collect static files.

.. code-block:: bash

$ docker exec -it project-compose_app_1 docker-entrypoint.sh bash
# django-admin migrate
# django-admin createsuperuser
# django-admin collectstatic --no-input