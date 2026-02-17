# Review Test Fixture

This file exists solely for testing the gh-review-comments skill.

## Function A

```python
def process_data(items):
    result = []
    for item in items:
        result.append(item * 2)
    return result
```

## Function B

```python
def fetch_user(user_id):
    response = requests.get(f"/users/{user_id}")
    return response.json()
```

## Function C

```python
def save_config(config):
    with open("config.json", "w") as f:
        json.dump(config, f)
```
