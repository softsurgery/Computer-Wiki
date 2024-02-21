```jsx
import React, { useState, useEffect } from "react";

const Typer = ({ speed = 100, children = " Introduce your text" }) => {
  const [idx, setidx] = useState(0);
  useEffect(() => {
    const timer = window.setInterval(() => setidx((v) => v + 1), speed);
    return () => window.clearInterval(timer);
  });

  return <div>{children.substr(0, idx) + " ■ "}</div>;
};

export default Typer;
```