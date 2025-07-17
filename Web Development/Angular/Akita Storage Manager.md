## Overview

This documentation covers a simple state management implementation using Akita in an Angular application. The implementation consists of three main parts:
1. **Store** - Defines the state structure and initial values
2. **Service** - Provides methods to interact with the store
3. **Component** - Consumes the state and triggers updates

## 1. Store

### Purpose

The `TestStore` class defines the structure of our state and its initial values using Akita's `Store` class.

```typescript
import { Store, StoreConfig } from '@datorama/akita';

export interface TestState {
  name: string;
}

export function createInitialState(): TestState {
  return {
    name: 'test123',
  };
}

@StoreConfig({ name: 'test' })
export class TestStore extends Store<TestState> {
  constructor() {
    super(createInitialState());
  }
}
```
### Key Features

- **TestState Interface**: Defines the shape of our state (currently just a `name` string)
- **createInitialState**: Factory function that returns the initial state
- **@StoreConfig**: Decorator that configures the store with a name ('test')
- **Extends Store**: Inherits all Akita store functionality

## 2. Service

### Purpose

The `TestService` acts as a facade between components and the store, providing clean methods to read and update state.

```typescript
import { Injectable } from '@angular/core';
import { TestState, TestStore } from './test.store';
import { Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class TestService {
  constructor(private testStore: TestStore) {}

  // Get observable of a specific field
  select<K extends keyof TestState>(field: K): Observable<TestState[K]> {
    return this.testStore._select(state => state[field]);
  }

  // Get current value (non-observable)
  get<K extends keyof TestState>(field: K): TestState[K] {
    const data = this.testStore.getValue();
    return data[field];
  }

  set<K extends keyof TestState>(field: K, value: TestState[K]) {
    this.testStore.update({ [field]: value });
  }
}
```
### Methods

| Method              | Description                                              | Returns         |
| ------------------- | -------------------------------------------------------- | --------------- |
| `select(field)`     | Returns an observable of a specific state property       | `Observable<T>` |
| `get(field)`        | Gets the current value of a state property (synchronous) | `T`             |
| `set(field, value)` | Updates a specific state property                        | `void`          |

### Key Features
- Type-safe access to state properties using generics    
- Both reactive (observable) and imperative (synchronous) access patterns
- Single responsibility for state interaction

## 3. Component

### Purpose
Demonstrates how to consume the state in an Angular component.

```typescript
import { Component } from '@angular/core';
import { TestService } from '../../store/test.service';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
})
export class TestComponent {
  name$: Observable<string>;

  constructor(private readonly testService: TestService) {
    this.name$ = this.testService.select('name');
  }

  updateName(name: string) {
    this.testService.set('name', name);
  }
}
```
### Template

```html
<div>
  <h1>Current name: {{ name$ | async }}</h1>
  <input #nameInput type="text" />
  <button (click)="updateName(nameInput.value)">Update Name</button>
</div>
```
### Key Features

- Uses observable state with `async` pipe for automatic subscription management
- Clean separation of concerns - component doesn't know about store implementation
- Simple UI binding and update mechanism

## Best Practices
1. **Store Structure**:    
    - Keep state interfaces simple and focused
    - Group related properties together
2. **Service Layer**:
    - Always access the store through service methods
    - Keep business logic in services, not components
3. **Components**:
    - Use observables with async pipe for templates
    - Avoid direct store access in components
    - Keep components focused on presentation