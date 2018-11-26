FROM mastertos/django:base
WORKDIR /code
ADD example /code
EXPOSE 8000
ENTRYPOINT ["python", "manage.py", "runserver", "0.0.0.0:8000"]