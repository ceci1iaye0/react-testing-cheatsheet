# react-hooks

## Basics

```typescript
// import { act, renderHook } from "@testing-library/react-hooks"; // React v17 and below
import { act, renderHook } from "@testing-library/react"; // React v18 onwards

describe("Test", () => {
  const initialProps = {
    closeModal: jest.fn();
  };

  afterEach(()=> {
    jest.clearAllMocks();
  })

  it("Test", () => {
    const { result } = useCustomHook(initialProps);

    act(()=> {
        result.current.handleSubmit();
    })

    expect(initialProps.closeModal).toHaveBeenCalled();
  });
});
```

## `unmount`

```typescript
import { act, renderHook } from "@testing-library/react";

describe("Test", () => {
  const initialProps = {
    closeModal: jest.fn();
  };

  afterEach(()=> {
    jest.clearAllMocks();
  })

  it("Test", () => {
    const { unmount } = useCustomHook(initialProps);

    unmount();

    expect(initialProps.closeModal).toHaveBeenCalled();
  });
});
```
