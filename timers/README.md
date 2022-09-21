# Timers

```typescript
it("Test", () => {
  jest.useFakeTimers();

  renderHook(() => useCustomHook());

  jest.advanceTimersByTime(5000);
  expect(mockFn).toHaveBeenCalled();

  jest.useRealTimers();
});
```
