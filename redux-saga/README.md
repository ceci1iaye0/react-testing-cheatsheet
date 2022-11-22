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

# Generator Function

```typescript
// index.ts
import { call } from "redux-saga/effects";

export function* getPost(postId: string) {
  return yield call("http://url.com", postId);
}

// index.test.ts
import { call } from "redux-saga-test-plan/matchers";

describe("getPost", () => {
  it("Should call API to get post", () => {
    const postId = "1";

    const getPostFn = getPost(postId);
    const result = getPostFn.next();
    expect(result.value).toEqual(call("http://url.com", postId));
  });
});
```

```typescript
// index.ts
import { call } from "redux-saga/effects";

export function* getPosts(postIds: string[]) {
  const res: any[] = [];

  for (const postId of postIds) {
    const res_ = yield getPost(postId);
    res.push(res_);
  }

  return res;
}

// index.test.ts
import { call } from "redux-saga-test-plan/matchers";

describe("getPosts", () => {
  it("Should call API to get post", () => {
    const postIds = ["1", "2"];

    const getPostsFn = getPosts(postIds);
    let result = getPostsFn.next();
    result = getPostsFn.next("mock res for postId = 1");
    result = getPostsFn.next("mock res for postId = 2");
    expect(result.value).toEqual([
      "mock res for postId = 1",
      "mock res for postId = 2",
    ]);
  });
});
```
