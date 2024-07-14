

- mixins
- extension

```dart
class User {
	
  String? _email;
  
  
  User({
    required String email,
  }) {
    this.email = email
  }
  
	set email (String value) {
    if (value.contains('@')) {
      _email = value
    } else {
      _email = null
    }
  }
}
```







