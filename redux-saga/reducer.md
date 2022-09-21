# Reducer

```typescript
import PostReducer, {
  containerId,
  fetchPosts,
  initialState,
  selector,
} from "../postReducer";

describe("PostReducer", () => {
  it("Should return the initial state by default", () => {
    expect(PostReducer(undefined, {})).toEqual(initialState);
  });

  it("Should update isLoading and error on fetchPosts.REQUEST", () => {
    const results = PostReducer(
      {
        postsData: [],
        isFetchPostsLoading: true,
        fetchPostsError: "error",
      },
      fetchPosts.request()
    );

    expect(results).toEqual({
      postsData: [],
      isFetchPostsLoading: false,
      fetchPostsError: "",
    });
  });

  it("Should update data and isLoading on fetchPosts.SUCCESS", () => {
    const payload = [{ postId: 1 }];
    const results = PostReducer(
      {
        postsData: [],
        isFetchPostsLoading: true,
        fetchPostsError: "",
      },
      fetchPosts.success(payload)
    );

    expect(results).toEqual({
      postsData: payload,
      isFetchPostsLoading: false,
      fetchPostsError: "",
    });
  });

  it("Should update data and isLoading on fetchPosts.FAILURE", () => {
    const payload = "error";
    const results = PostReducer(
      {
        postsData: [],
        isFetchPostsLoading: true,
        fetchPostsError: "",
      },
      fetchPosts.failure(payload)
    );

    expect(results).toEqual({
      postsData: [],
      isFetchPostsLoading: false,
      fetchPostsError: payload,
    });
  });

  it("Selector should return the correct result", () => {
    const text = "test";
    const selectedData = selector({ [containerId]: text });

    expect(selectData).toEqual(text);
  });
});
```
