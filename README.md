# navme by zfx.com

[![Pub](https://img.shields.io/pub/v/navme.svg)](https://pub.dev/packages/navme)


## Import

```yaml
navme: 0.9.0
```

```dart
import 'package:navme/navme.dart';
```

## Example use

```dart
 // config page
 class BooksListNavigate {
  // base path
  static String path = 'book';

  // config for configurate Router
  static RouteConfig routeConfig = RouteConfig(
    state: RouteState(uri: path.toUri()),
    // condition for using this page
    isThisPage: (RouteState state) {
      if ((state?.firstPath == path || state?.uri?.pathSegments?.isEmpty == true) && !state.hasParams) {
        return true;
      }
      return false;
    },
    // settigs from url
    settings: (RouteState state) {
      return null;
    },
    // get Page for Router
    page: ({RouteState state}) {
      return MaterialPage(key: const ValueKey('BooksListPage'), child: BooksListScreen.all(), name: 'BooksListScreen');
    },
  );
}
```

```dart
class NavmeRouterDelegate extends BaseRouterDelegate {
  NavmeRouterDelegate()
      : super(
          // base route
          initConfig: BooksListNavigate.routeConfig,
          configs: [
            // pages
            BookDetailsNavigate.routeConfig,
            BooksListNavigate.routeConfig,
            FadeNavigate.routeConfig,
            NestedNavigate.routeConfig,
            UnknownNavigate.routeConfig,
          ],
        );

  @override
  RouteState get currentConfiguration {
    return currentState;
  }

  // helper
  static NavmeRouterDelegate of(BuildContext context) {
    final delegate = Router.of(context).routerDelegate;
    if (delegate is NavmeRouterDelegate) {
      return delegate;
    }
    assert(() {
      throw FlutterError('Router operation requested with a context that does not include a NavmeRouterDelegate.\n');
    }(), 'Router operation requested with a context that does not include a NavmeRouterDelegate.\n');
    return null;
  }

  @override
  Widget build(BuildContext context) {
    return Navigator(
      key: navigatorKey,
      observers: [HeroController()], // THIS IS THE IMPORTANT LINE for Hero
      pages: buildPage(), // your stack pages
      onPopPage: (route, result) {
        if (!route.didPop(result)) {
          return false;
        }
        pop();
        return true;
      },
    );
  }
}
```

### Todo:

- return url
- nested url
- open dialog
