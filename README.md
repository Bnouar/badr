import { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";

const CLOUD_NAME = "dijudslgq";
const API_URL = `https://res.cloudinary.com/${CLOUD_NAME}/image/list/gallery.json`;

export default function Gallery() {
  const [images, setImages] = useState([]);

  useEffect(() => {
    const fetchImages = async () => {
      try {
        const response = await fetch(API_URL);
        if (!response.ok) {
          throw new Error("Failed to fetch images");
        }
        const data = await response.json();
        if (data.resources) {
          const urls = data.resources.map((img) => img.secure_url);
          setImages(urls);
        } else {
          console.error("No images found in Cloudinary response");
        }
      } catch (error) {
        console.error("Error fetching images from Cloudinary", error);
      }
    };
    fetchImages();
  }, []);

  return (
    <div className="grid grid-cols-2 md:grid-cols-4 gap-4 p-4">
      {images.length > 0 ? (
        images.map((url, index) => (
          <Card key={index} className="overflow-hidden rounded-xl shadow-lg">
            <CardContent className="p-0">
              <img src={url} alt={`Gallery image ${index}`} className="w-full h-60 object-cover" />
            </CardContent>
          </Card>
        ))
      ) : (
        <p className="text-center col-span-full">No images available</p>
      )}
    </div>
  );
}
