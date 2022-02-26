# flutter_event_transformer

플루터 이벤트 트랜스포머

## 프로젝트 설명

### 설정

> 증가버튼은 4초 이후 1증가, 감소버튼은 2초 이후 1감소

```dart
Future<void> _handleIncrementCounterEvent(event, emit) async {
await Future.delayed(const Duration(seconds: 4));
emit(state.copyWith(counter: state.counter + 1));
}

Future<void> _handleDecrementCounterEvent(event, emit) async {
await Future.delayed(const Duration(seconds: 2));
emit(state.copyWith(counter: state.counter - 1));
}
```

### 기본

> -1 >>> 0

```dart
CounterBloc() : super(CounterState.initial()) {
on<IncrementCounterEvent>(
    _handleIncrementCounterEvent,
);

on<DecrementCounterEvent>(
    _handleDecrementCounterEvent,
);
}
```

## Dependencies

```bash
flutter pub add equatable
flutter pub add flutter_bloc
flutter pub add bloc_concurrency
```
