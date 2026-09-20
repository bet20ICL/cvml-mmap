What is Ray?

The simplest possible application:

```python
model = load_model()

@app.post("/predict")
def predict(data):
    return model(data)
```

- exposes a single endpoint for model inference

