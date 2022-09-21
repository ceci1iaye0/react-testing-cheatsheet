# Axios

```typescript
import axios from "axios";

const mockAxios = axios as jest.Mocked<typeof axios>;

afterEach(() => {
  jest.clearAllMocks();
});

describe("Test", () => {
  beforeEach(() => {
    mockAxios.get.mockImplementation(() => {
      Promise.resolve({ data: [] });
    });

    mockAxios.post.mockImplementation(() => {
      Promise.reject(new Error());
    });
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  it("Test GET", () => {
    const res = await http.get("/api/posts");

    expect(mockAxios.get).toHaveBeenCalledWith("/api/posts");
  });

  it("Test POST", () => {
    const res = await http.post("/api/post", { postId: 1 });

    expect(mockAxios.post).toHaveBeenCalledWith("/api/post", { postId: 1 });
  });
});
```
