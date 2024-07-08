FROM python:3.11.3 

WORKDIR /app

RUN pip install poetry==1.7.0

COPY pyproject.toml ./

RUN poetry config virtualenvs.create false \
    && poetry install --no-interaction --no-ansi

COPY . .

EXPOSE 8000

RUN chmod +x ./prestart.sh

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]