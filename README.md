# React Testing

### Scripts

- Specify jest environment `--env jest-environment-jsdom-fourteen --bail`

```json
# package.json

{
    "scripts": {
        "test": "react-scripts test --testMatch=\"**/containers/**/__tests__/**/*\" --coverage --collectCoverageFrom=\"**/containers/**/*\""
    }
}
```

### `jest.mock`

```typescript
jest.mock("../DefaultExportComponent", () => () => <div />);

jest.mock("../NamedExportComponent", () => ({ NamedExport: () => <div /> }));

jest.mock(".../useCustomHook.tsx", () => () => jest.fn());

jest.mock(".../useCustomHook.tsx", () => () => ({
    handleOnChange: jest.fn();
}));
```

### `jest.spyOn`

```typescript
import * as useCustomHook from "../useCustomHook";

jest
  .spyOn(useCustomHook, "default")
  .mockReturnValue({ handleOnChange: jest.fn() }); // mock return value

jest
  .spyOn(useCustomHook, "default")
  .mockImplementation(() => ({ handleOnChange: jest.fn() })); // mock implementation
```

### Render

```typescript
import { render, screen } from "@testing-library/react";

test("Render component", () => {
  render(<Component />);

  expect(screen.getByTestId("component-id")).toBeInTheDocument();
});
```

```typescript
import { renderHook, act } from "@testing-library/react-hooks";

test("Render hook", () => {
  const { result } = renderHook(() => useCustomHook());

  act(() => result.current.handleOnChange());
});
```

## React Router

### `useNavigation` (v6)

```typescript
const mockNavigate = jest.fn();

jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useNavigate: () => mockNavigate,
}));

expect(mockNavigate).toHaveBeenCalledWith("/page", { state: {} });
```

### `useLocation` (v6)

```typescript
jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useLocation: () => ({ pathname: "", state: {} }),
}));
```

```typescript
const mockLocation = jest.fn();
jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useLocation: () => mockLocation(),
}));

beforeEach(() => {
  mockLocation.mockReturnValue({ state: {} });
});
```

```typescript
jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useLocation: jest.fn().mockImplementation(() => ({ pathname: "" })),
}));
```

### `useHistory` (v5)

```typescript
jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useHistory: () => ({
    location: { state: {} },
    push: jest.fn(),
  }),
}));
```

## `window.location`

```typescript
beforeEach(() => delete window.location);
afterEach(() => delete window.location);

window.location = { origin: "" };
```

## Redux-Saga

### Reducer

```typescript
import TodosReducer, {
  containerId,
  fetchTodos,
  initialState,
  selector,
} from "../reducer";

describe("Todos reducer", () => {
  test("Should return initial state by default", () => {
    expect(TodosReducer(undefined, {})).toEqual(initialState);
  });

  test("Should set values for isFetchTodosLoading and error on fetchTodos.REQUEST", () => {
    const resultState = TodosReducer(
      { isFetchTodosLoading: false, fetchTodosError: "Error" },
      fetchTodos.request()
    );

    expect(resultState).toEqual({
      isFetchTodosLoading: true,
      fetchTodosError: "",
    });
  });

  it("Should set values for todos and isFetchTodosLoading on fetchTodos.SUCCESS", () => {
    const payload = [];
    const resultState = TodosReducer(
      { todos: [], isFetchTodosLoading: true },
      fetchTodos.success(payload)
    );

    expect(resultState).toEqual({
      todos: payload,
      isFetchTodosLoading: false,
    });
  });

  it("Should set values for isFetchTodosLoading and fetchTodosError on fetchTodos.FAILURE", () => {
    const payload = "Error";
    const resultState = TodosReducer(
      { isFetchTodosLoading: true, fetchTodosError: "" },
      fetchTodos.failure(payload)
    );

    expect(resultState).toEqual({
      isFetchTodosLoading: false,
      fetchTodosError: payload,
    });
  });

  it("Selector should return correct result", () => {
    const text = "test";
    const selectedData = selector({ [containerId]: text });

    expect(selectedData).toEqual(text);
  });
});
```

### Sagas

```typescript
import { expectSaga } from "redux-saga-test-plan";
import { call, select } from "redux-saga-test-plan/matchers";
import { throwError } from "redux-saga-test-plan/providers";

import { fetchTodos, selector } from "../reducer";
import TodosSagas from "../sagas";
import * as api from "../shared/services/api";

describe("Todos sagas", () => {
  it("Should put success with response on successful fetchTodos", () => {
    const payload = {};
    const res = {};

    return expectSaga(TodosSagas)
      .provide([
        [select(selector), {}],
        [call(api.getTodos.get, payload), res],
      ])
      .put(fetchTodos.success(res))
      .dispatch(fetchTodos.request(payload))
      .silentRun();
  });

  it("Should put failure with error on unsuccessful fetchTodos", () => {
    const payload = {};
    const error = new Error();

    return expectSaga(TodosSagas)
      .provide([
        [select(selector), {}],
        [call(api.getTodos.get, payload), throwError(error)],
      ])
      .put(fetchTodos.failure(error))
      .dispatch(fetchTodos.request(payload))
      .silentRun();
  });
});
```

### `useDispatch`

```typescript
import * as reactRedux from "react-redux";

describe("Test", () => {
  let dispatch;

  beforeAll(() => {
    dispatch = jest.fn();
    jest.spyOn(reactRedux, "useDispatch").mockReturnValue(dispatch);
  });

  test("Should be able to dispatch", () => {
    const payload = {};
    expect(dispatch).toHaveBeenCalledWith(fetchTodos.request({ payload }));
  });
});
```

### `useSelector`

```typescript
import * as reactRedux from "react-redux";
import { containerId } from "../reducer";

describe("Test", () => {
  let selector;

  beforeEach(() => {
    selector =
      (values: {
        todos: [];
        isFetchTodosLoading: boolean;
        fetchTodosError: string;
      }) =>
      (
        state: (arg0: {
          TodosPage: {
            todos: [];
            isFetchTodosLoading: boolean;
            fetchTodosError: string;
          };
        }) => any
      ) =>
        state({ [containerId]: values });

    jest.spyOn(reactRedux, "useSelector").mockImplementation(
      selector({
        todos: [],
        isFetchTodosLoading: false,
        fetchTodosError: "",
      })
    );
  });
});
```

## Interval

```typescript
jest.useFakeTimers();
const { result } renderHook(() => useCustomEffects());

jest.advanceTimersByTime(5000);
expect(fetchTodos).toHaveBeenCalledTimes(1);

jest.useRealTimers();
```

## Promise

### Service

```typescript
import * as TodosService from "../TodosService";

jest
  .spyOn(TodosService, "getTodos")
  .mockResolvedValue({ status: "success", data: [] });

jest.spyOn(TodosService, "getTodos").mockRejectedValue(new Error());
```

### Axios

```typescript
import axios from "axios";

const mockAxios = axios as jest.Mocked<typeof axios>;

mockAxios.get.mockImplementation(() => {
  Promise.resolve({ data: [] });
});

mockAxios.post.mockImplementation(() => {
  Promise.reject(new Error());
});
```

## Modules

```typescript
import FileSaver from "file-saver";

jest.mock("file-saver", () => ({ saveAs: jest.fn() }));
expect(FileSaver.saveAs).toHaveBeenCalledTimes(1);
```

## User Interactions

### `querySelector`

```typescript
const { container } = render(<Component />);

container.querySelector("input[id='test']");
container.querySelector("input[value='test']");
container.querySelector("#test");
container.querySelector("[type='file']");
container.querySelector("div[id='test']");
```

### Dropdown

```typescript
const dropdown = container.querySelector("#dropdown-id");

fireEvent.keyDown(dropdown.firstChild, { key: "ArrowDown" });
fireEvent.click(screen.getByText("Next option"));

expect(mockOnSelectChange).toHaveBeenCalledTimes(1);
```

### Text Input

```typescript
fireEvent.change(screen.getByTestId("text-id"), {
  target: { value: "Test" },
});
```

### Button

```typescript
fireEvent.click(screen.getByTestId("button-id"));
```

## Templates

### useCustomStates

```typescript
import { act, renderHook } from "@testing-library/react-hooks";
import useCustomStates from "../useCustomStates";

describe("useCustomStates", () => {
  let result;

  beforeEach(() => {
    const customHook = renderHook(() => useCustomStates());
    result = customHook.result;
  });

  afterEach(() => jest.clearAllMocks());

  it("Handle id state", () => {
    expect(result.current.id).toBe("");

    act(() => {
      result.current.setId("test");
    });

    expect(result.current.id).toBe("test");
  });
});
```
