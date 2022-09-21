# Sagas

```typescript
import { expectSaga } from "redux-saga-test-plan";
import { call, select } from "redux-saga-test-plan/matchers";
import { throwError } from "redux-saga-test-plan/providers";

import { fetchPosts, selector } from "../postReducer";
import PostSagas from "../postSagas";

import * as api from "../api";

describe("PostSagas", () => {
  describe("fetchPostsSaga", () => {
    it("Should put success with response", () => {
      const res = { data: {} };

      return expectSaga(PostSagas)
        .provide([
          [select(selector), {}],
          [call(api.posts.get), res],
        ])
        .put(fetchPosts.success(res.data))
        .dispatch(fetchPosts.request())
        .run();
    });

    it("Should put failure with error", () => {
      const err = new Error("error");

      return expectSaga(PostSagas)
        .provide([
          [select(selector), {}],
          [call(api.posts.get), throwError(err)],
        ])
        .put(fetchPosts.failure(err))
        .dispatch(fetchPosts.request())
        .run();
    });
  });
});
```
