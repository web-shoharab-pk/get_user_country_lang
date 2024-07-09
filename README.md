# get_user_country_lang
```
https://api.ipregistry.co/?key=tryout
```
import React, { useState, useEffect } from 'react';

const UserLocationLanguage = () => {
  const [country, setCountry] = useState('');
  const [language, setLanguage] = useState('');

  useEffect(() => {
    // Get the default language
    const userLanguage = navigator.language || navigator.userLanguage;
    setLanguage(userLanguage);

    // Function to fetch country from a given URL
    const fetchCountryFromURL = async (url) => {
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`Network response was not ok: ${response.statusText}`);
        }
        const data = await response.json();
        return data.country_name;
      } catch (error) {
        console.error(`Error fetching the user country from ${url}:`, error);
        throw error;
      }
    };

    // Get the user's country using multiple services
    const fetchCountry = async () => {
      const services = [
        'https://ipapi.co/json/',
        'https://ipinfo.io/json?token=YOUR_TOKEN',
        'https://geoip-db.com/json/'
      ];

      for (const service of services) {
        try {
          const country = await fetchCountryFromURL(service);
          setCountry(country);
          break; // Exit the loop if successful
        } catch (error) {
          // Continue to the next service if one fails
        }
      }
    };

    fetchCountry();
  }, []);

  return (
    <div>
      <p>User's Default Language: {language}</p>
      <p>User's Country: {country}</p>
    </div>
  );
};

export default UserLocationLanguage;
