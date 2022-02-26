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

### transformer(droppable)

> 계속 누르면, 겹치면 무시하고, 수행됨.

```dart
on<DecrementCounterEvent>(
    _handleDecrementCounterEvent,
    transformer: droppable(),
);
```

### transformer(restartable)

> 계속 누르면, 겹치면 이전 처리를 무시하고 재시작함.

```dart
on<DecrementCounterEvent>(
    _handleDecrementCounterEvent,
    transformer: restartable(),
);
```

### transformer(sequential)

> 계속 누르면, 겹치면, 겹친만큼 순차적으로  딜레이후 1씩 감소함.

```dart
on<DecrementCounterEvent>(
    _handleDecrementCounterEvent,
    transformer: sequential(),
);
```

### CounterEvent

> 이벤트를 하나로 묶어서 처리 (sequential)
> 4초뒤에 1이 증가되고, 이후 2초뒤에 1이 증가됨.

```dart
on<CounterEvent>(
    (event, emit) async {
        if (event is IncrementCounterEvent) {
            await _handleIncrementCounterEvent(event, emit);
        } else if (event is DecrementCounterEvent) {
            await _handleDecrementCounterEvent(event, emit);
        }
    },
    transformer: sequential(),
);
```

## Dependencies

```bash
flutter pub add equatable
flutter pub add flutter_bloc
flutter pub add bloc_concurrency
```
