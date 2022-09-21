# Firing Events

## `querySelector`

```typescript
import { render } from "@testing-library/react";

it("Test", () => {
  const { container } = render(<Component />);

  container.querySelector("input[id='test']");
  container.querySelector("div[id='test']");
  container.querySelector("#test");
  container.querySelector("input[value='test']");
  container.querySelector("[type='file']");
});
```

## Dropdown

```typescript
import { fireEvent, render, screen } from "@testing-library/react";

it("Test", () => {
  const { container } = render(<Component />);

  const dropdown = container.querySelector("#dropdown-id");

  fireEvent.keyDown(dropdown.firstChild, { key: "ArrowDown" });
  fireEvent.click(screen.getByText("next option"));

  expect(mockOnChange).toHaveBeenCalled();
});
```

## Text Input

```typescript
import { fireEvent, render, screen } from "@testing-library/react";

it("Test", () => {
  const { container } = render(<Component />);

  fireEvent.change(screen.getByTestId("text-field-id"), {
    target: { value: "some value" },
  });
});
```

## Button

```typescript
import { fireEvent, render, screen } from "@testing-library/react";

it("Test", () => {
  const { container } = render(<Component />);

  fireEvent.click(screen.getByTestId("btn-id"));
});
```
