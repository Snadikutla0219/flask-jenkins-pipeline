# flask-jenkins-pipeline

A simple Flask app with health check endpoints.

## Usage

1. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

2. Run the app:
   ```
   python app.py
   ```

The app will run on `http://0.0.0.0:5000`.

### Routes

- `GET /`: Returns a welcome JSON message.
- `GET /health`: Returns a health status JSON with 200 status code.

## Testing

Run the tests with:
```
pytest test_app.py
```
