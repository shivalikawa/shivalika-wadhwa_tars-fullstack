import React, { useState, useEffect } from "react";
import Unsplash, { toJson } from "unsplash-js";

const unsplash = createApi({
  accessKey: "YOUR_UNSPLASH_ACCESS_KEY",
});

const App = () => {
  const [photos, setPhotos] = useState([]);
  const [searchQuery, setSearchQuery] = useState("");

  useEffect(() => {
    const fetchPhotos = async () => {
      const response = await unsplash.photos.listPhotos({
        query: searchQuery,
        page: 1,
        perPage: 20,
      });
      const photosData = await toJson(response);
      setPhotos(photosData.results);
    };

    fetchPhotos();
  }, [searchQuery]);

  const handleSearch = (event) => {
    setSearchQuery(event.target.value);
  };

  const handlePhotoClick = (photo) => {
    // Display the photo in a popup modal
    // ...
  };

  return (
    <div>
      <input
        type="text"
        placeholder="Search photos"
        onChange={handleSearch}
      />
      <div className="photo-grid">
        {photos.map((photo) => (
          <div className="photo-card" key={photo.id}>
            <img src={photo.urls.small} alt={photo.alt_description} />
            <div className="photo-info">
              <p>{photo.user.name}</p>
              <p>{photo.likes} likes</p>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};

export default App;
