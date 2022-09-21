# React Router

_For react router v6_

## `useNavigate`

```typescript
const mockNavigate = jest.fn();

jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useNavigate: () => mockNavigate,
}));

it("Test", () => {
  expect(mockNavigate).toHaveBeenCalledWith("/page", { state: {} });
});
```

## `useLocation`

```typescript
jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useLocation: () => ({ pathname: "/page", state: {} }),
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

## `useParams`

```typescript
jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useParams: () => ({ id: {} }),
}));
```
