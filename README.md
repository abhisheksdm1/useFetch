````
// src/hooks/useFetch.js

import { useState, useEffect, useCallback } from "react";

export function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const apiUrl = import.meta.env.VITE_API_URL;

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await fetch(`${apiUrl}/${url}`);
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      const json = await response.json();
      setData(json);
    } catch (err) {
      setError(err.message || "Something went wrong");
    } finally {
      setLoading(false);
    }
  }, [url]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  return { data, loading, error, refetch: fetchData };
}
````
`-------------------------------------

````

  const { data, loading, error, refetch } = useFetch("LoyaltyProgram/approval");
  const apiData = Array.isArray(data?.data) ? data.data : [];
  const [id, setId] = React.useState(null);
  const [type, setType] = React.useState(null);
  const [status, setStatus] = React.useState(null);
  const [actionLoading, setActionLoading] = React.useState(false);
  const [localData, setLocalData] = React.useState(apiData);
  React.useEffect(() => {
    setLocalData(apiData);
  }, [apiData]);

  const {
    putData,
    loading: putLoading,
    error: putError,
  } = usePut("approvalCenter/update-status");

  const handleStatus = async (status, id, type) => {
    setActionLoading(true);
    try {
      await putData({
        status,
        id,
        type,
      });
      await refetch();
      // Optionally, refresh data or show feedback
    } catch (err) {
      // Handle error (show feedback if needed)
    } finally {
      setActionLoading(false);
    }
  };
````
