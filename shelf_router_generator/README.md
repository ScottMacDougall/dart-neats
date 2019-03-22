Shelf Router Generator
======================

[Shelf](https://pub.dartlang.org/packages/shelf) makes it easy to build web
applications in Dart by composing request handlers. The `shelf_router` package
offers a request router for Shelf. this package enables generating a
`shelf_route.Router` from annotations in code.

**Disclaimer:** This is not an officially supported Google product.

## Example

```dart
import 'package:shelf/shelf.dart';
import 'package:shelf_router/shelf_router.dart';

part 'userservice.g.dart';

class UserService {
  final DatabaseConnection connection;
  UserService(this.connection);

  @Route.get('/users/')
  Future<Response> listUsers(Request request) async {
    return Response.ok('["user1"]');
  }

  @Route.get('/users/<userId>')
  Future<Response> fetchUser(Request request, String userId) async {
    if (userId == 'user1') {
      return Response.ok('user1');
    }
    return Response.notFound('no such user');
  }

  // Create router using the generate function defined in 'userservice.g.dart'.
  Router get router => _$UserServiceRouter(this);
}

void main() async {
  // You can setup context, database connections, cache connections, email
  // services, before you create an instance of your service.
  var connection = await DatabaseConnection.connect('localhost:1234');

  // Create an instance of your service, usine one of the constructors you've
  // defined.
  var service = UserService(connection);
  // Service request using the router, note the router can also be mounted.
  var router = service.router;
  var server = await io.serve(router.handler, 'localhost', 8080);
}
```

