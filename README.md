# abn
import React, { useState, useEffect } from 'react';
import ClipLoader from 'react-spinners/ClipLoader'; 
import vedio from "../assets/imgs/login.mp4";

const Home = () => {
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Simulate a loading delay (e.g., for data fetching or image loading)
    const timer = setTimeout(() => {
      setLoading(false);
    }, 2000); // Adjust the delay as needed

    return () => clearTimeout(timer); 
  }, []);

  return (
    <div className="relative overflow-hidden bg-gray-100">
      {loading ? (
        <div className="flex items-center justify-center h-screen bg-white">
          <ClipLoader color="#0000ff" size={50} />
        </div>
      ) : (
        <>
          {/* Hero Section */}
          <section className="relative h-screen bg-cover bg-center text-white flex items-center justify-center p-6">
            {/* Full-Screen Video with z-index */}
            <video autoPlay loop muted className="absolute inset-0 w-full h- object-cover z-10">
              <source src={vedio} type="video/mp4" />
              Your browser does not support the video tag.
            </video>

           
          </section>
<br />
<br />
<br />
<br />
          {/* Features Section */}
          <section className="py-16 text-center">
            <h2 className="text-4xl font-semibold mb-8">Why Choose Us?</h2>
            <div className="flex flex-wrap justify-center gap-8">
              <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
                <h3 className="text-2xl font-bold mb-4">Premium Quality</h3>
                <p>Our curtains are made from the highest quality materials to ensure durability and style.</p>
              </div>
              <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
                <h3 className="text-2xl font-bold mb-4">Custom Designs</h3>
                <p>Choose from a wide range of designs and fabrics to match your unique style.</p>
              </div>
              <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
                <h3 className="text-2xl font-bold mb-4">Fast Delivery</h3>
                <p>Enjoy prompt delivery and hassle-free installation services.</p>
              </div>
            </div>
          </section>

          {/* Product Gallery Section */}
          <section className="py-16 bg-gray-200">
            <h2 className="text-4xl font-semibold text-center mb-8">Our Collection</h2>
            <div className="flex flex-wrap justify-center gap-8">
              {/* Example Product Card */}
              <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
                <img className="w-full h-56 object-cover rounded-lg mb-4" src="path-to-your-product-image.jpg" alt="Product Name" />
                <h3 className="text-2xl font-bold mb-4">Product Name</h3>
                <p>Description of the product goes here.</p>
                <p className="mt-4 font-bold">$99.99</p>
                <button className="mt-4 px-6 py-2 bg-green-600 hover:bg-green-500 text-white rounded-lg text-lg transition duration-300">
                  Add to Cart
                </button>
              </div>
              {/* Add more product cards as needed */}
            </div>
          </section>

          {/* Testimonials Section */}
          <section className="py-16 text-center">
            <h2 className="text-4xl font-semibold mb-8">What Our Customers Say</h2>
            <div className="flex flex-wrap justify-center gap-8">
              <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
                <p>"The curtains are absolutely beautiful and the service was excellent!"</p>
                <span className="block mt-4 font-bold">- Alice M.</span>
              </div>
              <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
                <p>"I am very impressed with the quality and design. Highly recommend!"</p>
                <span className="block mt-4 font-bold">- Brian T.</span>
              </div>
            </div>
          </section>
        </>
      )}
    </div>
  );
};

export default Home;
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import ClipLoader from 'react-spinners/ClipLoader';
import { useDispatch } from 'react-redux';
import { addItemToCart } from '../redux/features/cartSlice';
import curtains from "../assets/imgs/curtain.jpg";  

const sharedClasses = {
  card: 'bg-card p-4 rounded-lg shadow-md',
  button: 'p-2 rounded-lg',
  primaryButton: 'bg-primary text-primary-foreground hover:bg-primary/80',
  secondaryButton: 'bg-secondary text-secondary-foreground hover:bg-secondary/80',
  formCheckbox: 'form-checkbox',
};

const FilterCheckbox = ({ label, checked, onChange }) => (
  <label className="inline-flex items-center">
    <input
      type="checkbox"
      checked={checked}
      onChange={onChange}
      className={sharedClasses.formCheckbox}
    />
    <span className="ml-2">{label}</span>
  </label>
);

const FilterSection = ({ filters, handleFilterChange, handleClearFilters }) => (
  <div className="bg-white p-6 rounded-lg shadow-md mb-8">
    <h2 className="text-2xl font-semibold mb-4">Filter Products</h2>

    <div className="mb-4">
      <h3 className="font-medium text-lg">Size</h3>
      <label className="block mt-2">Width</label>
      <input
        type="range"
        name="width"
        min="100"
        max="300"
        value={filters.width}
        onChange={handleFilterChange}
        className="w-full"
      />
      <label className="block mt-2">Height</label>
      <input
        type="range"
        name="height"
        min="100"
        max="300"
        value={filters.height}
        onChange={handleFilterChange}
        className="w-full"
      />
    </div>

    <div className="mb-4">
      <h3 className="font-medium text-lg">Availability</h3>
      <FilterCheckbox
        label="In Stock"
        checked={filters.inStock}
        onChange={() => handleFilterChange({ target: { name: 'inStock', value: !filters.inStock } })}
      />
    </div>

    <div className="mb-4">
      <h3 className="font-medium text-lg">Product Category</h3>
      <FilterCheckbox
        label="Curtains & Drapes"
        checked={filters.category.includes('Curtains & Drapes')}
        onChange={() =>
          handleFilterChange({ target: { name: 'category', value: 'Curtains & Drapes' } })
        }
      />
      <ul className="mt-2">
        <li>
          <FilterCheckbox
            label="Pinch Pleat Curtains"
            checked={filters.subCategory.includes('Pinch Pleat Curtains')}
            onChange={() =>
              handleFilterChange({ target: { name: 'subCategory', value: 'Pinch Pleat Curtains' } })
            }
          />
        </li>
        <li>
          <FilterCheckbox
            label="Ripple Fold Curtains"
            checked={filters.subCategory.includes('Ripple Fold Curtains')}
            onChange={() =>
              handleFilterChange({ target: { name: 'subCategory', value: 'Ripple Fold Curtains' } })
            }
          />
        </li>
        <li>
          <FilterCheckbox
            label="Blackout Curtain"
            checked={filters.subCategory.includes('Blackout Curtain')}
            onChange={() =>
              handleFilterChange({ target: { name: 'subCategory', value: 'Blackout Curtain' } })
            }
          />
        </li>
      </ul>
    </div>

    <div className="mb-4">
      <h3 className="font-medium text-lg">Color</h3>
      <ul className="mt-2">
        <li>
          <FilterCheckbox
            label="White"
            checked={filters.color.includes('White')}
            onChange={() => handleFilterChange({ target: { name: 'color', value: 'White' } })}
          />
        </li>
        <li>
          <FilterCheckbox
            label="Beige"
            checked={filters.color.includes('Beige')}
            onChange={() => handleFilterChange({ target: { name: 'color', value: 'Beige' } })}
          />
        </li>
      </ul>
    </div>

    <button
      className={`${sharedClasses.primaryButton} ${sharedClasses.button} w-full`}
      onClick={handleClearFilters}
    >
      Clear All
    </button>
  </div>
);

const ProductCard = ({ imageUrl, alt, price, description, onAddToCart }) => (
  <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
    <img src={imageUrl} alt={alt} className="w-full h-56 object-cover rounded-lg mb-4" />
    <h3 className="text-2xl font-bold mb-4">From ${price.toFixed(2)}</h3>
    <p>{description}</p>
    <p className="mt-4 font-bold">${price.toFixed(2)}</p>
    <button
      onClick={onAddToCart}
      className="mt-4 px-6 py-2 bg-green-600 hover:bg-green-500 text-white rounded-lg text-lg transition duration-300"
    >
      Add to Cart
    </button>
  </div>
);





const ProductGallery = ({ products, handleAddToCart }) => (
  <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
    {products.map((product) => (
      <ProductCard
        key={product._id}
        imageUrl={product.imageUrl}
        alt={product.name}
        price={product.price}
        description={product.description}
        onAddToCart={() => handleAddToCart(product)}
      />
    ))}
  </div>
);

const CurtainsDrapes = () => {
  const [products, setProducts] = useState([]);
  const [filters, setFilters] = useState({
    width: 150,
    height: 150,
    inStock: false,
    category: [],
    subCategory: [],
    color: [],
  });
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const dispatch = useDispatch();

  useEffect(() => {
    const fetchProducts = async () => {
      setLoading(true);
      try {
        const response = await axios.get('http://localhost:5000/api/products/category/curtains-drapes', {
          params: filters,
        });
        setProducts(response.data);
      } catch (error) {
        setError('Failed to load products');
      } finally {
        setLoading(false);
      }
    };

    fetchProducts();
  }, [filters]);

  const handleAddToCart = (product) => {
    dispatch(addItemToCart(product));
  };

  const handleFilterChange = (e) => {
    const { name, value } = e.target;
    setFilters((prevFilters) => {
      if (Array.isArray(prevFilters[name])) {
        // Handle array filters (like categories and colors)
        const newFilterValues = prevFilters[name].includes(value)
          ? prevFilters[name].filter((item) => item !== value)
          : [...prevFilters[name], value];
        return { ...prevFilters, [name]: newFilterValues };
      } else {
        // Handle non-array filters (like width, height, inStock)
        return { ...prevFilters, [name]: value };
      }
    });
  };

  const handleClearFilters = () => {
    setFilters({
      width: 150,
      height: 150,
      inStock: false,
      category: [],
      subCategory: [],
      color: [],
    });
  };

  const HeroSection = () => (
    <section
      className="relative bg-cover bg-center text-white flex items-center justify-center p-6 sm:p-12"
      style={{ backgroundImage: `url(${curtains})`, height: '500px' }}
    >
      <div className="absolute inset-0 bg-black opacity-50"></div>
      <div className="relative z-10 text-center">
        <h1 className="text-3xl sm:text-5xl font-bold">Curtains & Drapes</h1>
        <p className="mt-4 text-base sm:text-lg">Enhance your home's interior with our elegant curtains and drapes.</p>
      </div>
    </section>
  );

  if (loading)
    return (
      <div className="flex items-center justify-center h-screen bg-white">
        <ClipLoader color="#0000ff" size={50} />
      </div>
    );

  if (error)
    return <p className="text-center mt-10 text-red-600">{error}</p>;

  return (
    <div className="font-sans bg-white text-gray-900">
      <HeroSection />
      <div className="container mx-auto px-4 py-8">
        <div className="md:flex gap-8">
          <div className="md:w-1/4">
            <FilterSection
              filters={filters}
              handleFilterChange={handleFilterChange}
              handleClearFilters={handleClearFilters}
            />
          </div>
          <div className="md:w-3/4">
            <ProductGallery products={products} handleAddToCart={handleAddToCart} />
          </div>
        </div>
      </div>
    </div>
  );
};

export default CurtainsDrapes;
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import ClipLoader from 'react-spinners/ClipLoader';
import { useDispatch } from 'react-redux';
import curtain from "../assets/imgs/curtain.jpg";
import { addItemToCart } from '../redux/features/cartSlice';

const BlindsShades = () => {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [filter, setFilter] = useState({ minPrice: 0, maxPrice: 1000 }); // Added filter state
  const dispatch = useDispatch();

  useEffect(() => {
    const fetchProducts = async () => {
      try {
        const response = await axios.get('http://localhost:5000/api/products/category/blinds-shades');
        setProducts(response.data);
      } catch (error) {
        setError('Failed to load products');
      } finally {
        setLoading(false);
      }
    };

    fetchProducts();
  }, []);

  const handleAddToCart = (product) => {
    dispatch(addItemToCart(product));
  };

  // Filter products based on price range
  const filteredProducts = products.filter(
    (product) => product.price >= filter.minPrice && product.price <= filter.maxPrice
  );

  const handleFilterChange = (e) => {
    const { name, value } = e.target;
    setFilter((prevFilter) => ({
      ...prevFilter,
      [name]: value,
    }));
  };

  const HeroSection = () => (
    <section className="relative h-screen bg-cover bg-center text-white flex items-center justify-center p-6" style={{ backgroundImage: `url(${curtain})` }}>
      <div className="absolute inset-0 bg-black opacity-50"></div>
      <div className="relative z-10 text-center">
        <h1 className="text-5xl font-bold">Blinds & Shades</h1>
        <p className="mt-4 text-lg">Discover our range of stylish blinds and shades for every room.</p>
      </div>
    </section>
  );

  const FeaturesSection = () => (
    <section className="py-16 text-center">
      <h2 className="text-4xl font-semibold mb-8">Features</h2>
      <div className="flex flex-wrap justify-center gap-8">
        <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
          <h3 className="text-2xl font-bold mb-4">Light Control</h3>
          <p>Control the amount of light entering your room with our customizable blinds and shades.</p>
        </div>
        <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
          <h3 className="text-2xl font-bold mb-4">Privacy</h3>
          <p>Enjoy enhanced privacy with our premium quality blinds.</p>
        </div>
        <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
          <h3 className="text-2xl font-bold mb-4">Easy Installation</h3>
          <p>Quick and easy installation for all our blinds and shades.</p>
        </div>
      </div>
    </section>
  );

  const ProductGallery = ({ products, handleAddToCart }) => (
    <section className="py-16">
      <h2 className="text-4xl font-semibold text-center mb-8">Product Gallery</h2>
      <div className="flex flex-wrap justify-center gap-8">
        {products.length > 0 ? (
          products.map((product) => (
            <div key={product._id} className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
              <img
                className="w-full h-56 object-cover rounded-lg mb-4"
                src={product.imageUrl}
                alt={product.name}
              />
              <h3 className="text-2xl font-bold mb-4">{product.name}</h3>
              <p>{product.description}</p>
              <p className="mt-4 font-bold">${product.price.toFixed(2)}</p>
              <button
                onClick={() => handleAddToCart(product)}
                className="mt-4 px-6 py-2 bg-green-600 hover:bg-green-500 text-white rounded-lg text-lg transition duration-300"
              >
                Add to Cart
              </button>
            </div>
          ))
        ) : (
          <p className="text-center text-gray-500">No products available.</p>
        )}
      </div>
    </section>
  );

  const FilterSection = () => (
    <section className="py-8 text-center">
      <h2 className="text-2xl font-semibold mb-4">Filter Products</h2>
      <div className="flex justify-center gap-4 mb-8">
        <div>
          <label className="block text-lg font-medium mb-2">Min Price</label>
          <input
            type="number"
            name="minPrice"
            value={filter.minPrice}
            onChange={handleFilterChange}
            className="border border-gray-300 rounded-lg p-2"
          />
        </div>
        <div>
          <label className="block text-lg font-medium mb-2">Max Price</label>
          <input
            type="number"
            name="maxPrice"
            value={filter.maxPrice}
            onChange={handleFilterChange}
            className="border border-gray-300 rounded-lg p-2"
          />
        </div>
      </div>
    </section>
  );

  const TestimonialsSection = () => (
    <section className="py-16 bg-gray-100 text-center">
      <h2 className="text-4xl font-semibold mb-8">What Our Customers Say</h2>
      <div className="flex flex-wrap justify-center gap-8">
        <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
          <p>
            "These blinds have added a lot of style and functionality to my living room."
          </p>
          <span className="block mt-4 font-bold">- Jane S.</span>
        </div>
        <div className="w-80 p-6 bg-white shadow-lg rounded-lg mb-8">
          <p>
            "I can now control the lighting in my office with just a remote. Love it!"
          </p>
          <span className="block mt-4 font-bold">- Mike L.</span>
        </div>
      </div>
    </section>
  );

  if (loading)
    return (
      <div className="flex items-center justify-center h-screen bg-white">
        <ClipLoader color="#0000ff" size={50} />
      </div>
    );

  if (error)
    return <p className="text-center mt-10 text-red-600">{error}</p>;

  return (
    <div className="font-sans bg-white text-gray-900">
      <HeroSection />
      <FeaturesSection />
      <FilterSection /> {/* Filter section added */}
      <ProductGallery products={filteredProducts} handleAddToCart={handleAddToCart} />
      <TestimonialsSection />
    </div>
  );
};

export default BlindsShades;

