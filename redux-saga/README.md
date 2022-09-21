# Dispatch

## Native

```typescript
import * as reactRedux from "react-redux";

describe("Test", () => {
  let dispatch;

  beforeEach(() => {
    dispatch = jest.fn();
    jest.spyOn(reactRedux, "useDispatch").mockReturnValue(dispatch);
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  it("Should dispatch fetchPosts.REQUEST", () => {
    expect(dispatch).toHaveBeenCalledWith(fetchPosts.request());
  });
});
```

## mock-react-redux

```typescript
import mockReactRedux from "mock-react-redux";

describe("Test", () => {
  let dispatch;

  beforeEach(() => {
    const initMockReactRedux = mockReactRedux();
    dispatch = initMockReactRedux.dispatch;
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  it("Should dispatch fetchPosts.REQUEST", () => {
    expect(dispatch).toHaveBeenCalledWith(fetchPosts.request());
  });
});
```

# Selector

## Native

```typescript
import * as reactRedux from "react-redux";
import { containerId } from "../postReducer";

describe("Test", () => {
  const selector =
    (values: {
      postsData: [];
      isFetchPostsLoading: boolean;
      fetchPostsError: string;
    }) =>
    (
      state: (arg0: {
        PostReducer: {
          postsData: [];
          isFetchPostsLoading: boolean;
          fetchPostsError: string;
        };
      }) => any
    ) =>
      state({ [containerId]: values });

  beforeEach(() => {
    jest.spyOn(reactRedux, "useSelector").mockImplementation(
      selector({
        postsData: [],
        isFetchPostsLoading: false,
        fetchPostsError: "",
      })
    );
  });

  afterEach(() => {
    jest.clearAllMocks();
  });
});
```

## mock-react-redux

```typescript
import mockReactRedux from "mock-react-redux";
import { selector } from "../postReducer";

describe("Test", () => {
  beforeEach(() => {
    const initMockReactRedux = mockReactRedux();
    initMockReactRedux.giveMock(
      selector,
      jest.fn().mockReturnValue({
        postsData: [],
        isFetchPostsLoading: false,
        fetchPostsError: "",
      })
    );
  });

  afterEach(() => {
    jest.clearAllMocks();
  });
});
```
