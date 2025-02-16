# YAML Basics

YAML (YAML Ain't Markup Language) is a human-readable data serialization format commonly used for configuration files and data exchange between languages with different data structures.

## Basic Syntax
- Uses indentation (spaces, not tabs) to define structure.
- Key-value pairs use `:`.
- Lists use `-`.
- Dictionaries (maps) use key-value pairs.

## Examples

### 1. Key-Value Pair
```yaml
database: mysql
username: admin
password: secret
```

### 2. Array List
```yaml
fruits:
  - apple
  - banana
  - cherry
```

### 3. Dictionary (Map)
```yaml
person:
  name: John Doe
  age: 30
  city: New York
```

### 4. Nested Structures
```yaml
server:
  name: myserver
  ip: 192.168.1.1
  location:
    country: USA
    city: San Francisco
```

### 5. Inline List and Map
```yaml
colors: [red, green, blue]
coordinates: {x: 10, y: 20}
```

## Additional Features

### Comments
```yaml
# This is a comment
version: 1.0  # Inline comment
```

### Multi-line Strings
```yaml
description: |
  This is a multi-line
  string in YAML.
```

### Boolean Values
```yaml
enabled: true
debug: false
```

### Null Values
```yaml
optional_field: null
```

### Using Aliases (References)
```yaml
defaults: &default_settings
  timeout: 30
  retries: 3

server1:
  <<: *default_settings
  host: server1.example.com
```

## Conclusion
YAML is widely used in configuration management, Kubernetes manifests, CI/CD pipelines, and data serialization. It is easy to read and supports complex data structures effectively.

