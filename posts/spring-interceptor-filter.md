## interceptor & filter

### interceptor 의 `preHandle(..)` 의 return boolean 의미

`preHandle()` 메서드에서 `return` 값은 `boolean`

보통 redirect 로직을 수행할 때, `return false`를 반환한다.

`return false` 시 `postHandle()` 메서드를 수행하지 않게 됨