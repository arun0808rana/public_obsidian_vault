
```javascript
import { useState, useEffect, useCallback } from 'react';
import axios from 'axios';

const useAxios = (url, options = {}, enabled = true) => {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  const fetchData = useCallback(async () => {
    if (!enabled) {
      return;
    }

    setLoading(true);
    try {
      const response = await axios(url, options);
      setData(response.data);
      setError(null);
    } catch (err) {
      setError(err);
    } finally {
      setLoading(false);
    }
  }, [url, options, enabled]);

  useEffect(() => {
    if (enabled) {
      fetchData();
    }
  }, [fetchData, enabled]);

  return { data, error, loading, refetch: fetchData };
};

export default useAxios;

```
