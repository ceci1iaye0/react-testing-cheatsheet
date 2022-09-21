# React Testing Library

## Setup

- Specify jest environment `--env jest-environment-jsdom-fourteen`

```json
# package.json

{
  "scripts": {
      "test": "react-scripts test --testMatch=\"**/containers/**/__tests__/**/*\" --coverage --collectCoverageFrom=\"**/containers/**/*\" --bail --detectOpenHandles"
  },
  "jest": {
    "collectCoverageFrom": [
      "src/**/*.{ts,tsx}",
      "!<rootDir>/node_modules/"
    ]
  }
}
```

## `jest.mock`

```typescript
jest.mock("../DefaultExportComponent", () => () => <div />);

jest.mock("../NamedExportComponent", () => ({ NamedExport: () => <div /> }));

jest.mock(".../useCustomHook.tsx", () => () => jest.fn());

jest.mock(".../useCustomHook.tsx", () => () => ({
    handleOnChange: jest.fn();
}));
```

## `jest.spyOn`

```typescript
import * as useCustomHook from "../useCustomHook";

jest
  .spyOn(useCustomHook, "default")
  .mockReturnValue({ handleOnChange: jest.fn() }); // mock return value

jest
  .spyOn(useCustomHook, "default")
  .mockImplementation(() => ({ handleOnChange: jest.fn() })); // mock implementation
```

## `window.location`

```typescript
beforeEach(() => delete window.location);
afterEach(() => delete window.location);

window.location = { origin: "" };
```

## Modules

```typescript
import FileSaver from "file-saver";

jest.mock("file-saver", () => ({ saveAs: jest.fn() }));
expect(FileSaver.saveAs).toHaveBeenCalledTimes(1);
```
