Overview

Dream is a full-stack web application designed for seamless home rental management. It enables users to explore available homes, make bookings, and manage their reservations—all in one place. The platform is built using the MERN stack (MongoDB, Express.js, React.js, Node.js), ensuring a modern and efficient user experience.

✨ Features

🔐 User Authentication – Secure signup, login, and logout functionality

🏠 Browse Homes – View all available rental listings

🎯 Advanced Filtering – Search homes by location, price, and amenities

📅 Booking System – Book and manage home rentals easily

👤 User Dashboard – View and manage your bookings

⚙️ Admin Panel – Manage homes, bookings, and users with full control

🛠️ Tech Stack

Frontend: React.js, Redux

Backend: Node.js, Express.js

Database: MongoDB

Other Tools: HTML, CSS, JavaScript, Bootstrap

🗄️ Database Structure

Dream uses MongoDB (NoSQL) for efficient and scalable data storage.
Each collection is designed for modularity and optimized querying using Mongoose models.

1. Users Collection

Stores information about all registered users — including guests, hosts, and admins.

{
  _id: ObjectId,
  name: String,
  email: { type: String, unique: true },
  password: String, // Encrypted with bcrypt
  role: { type: String, enum: ["user", "host", "admin"], default: "user" },
  avatar: String, // Profile image URL
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
}


Relations:

One user can have multiple bookings

A host user can manage multiple properties

2. Properties Collection

Contains details of all listed rental homes.

{
  _id: ObjectId,
  title: String,
  description: String,
  location: String,
  pricePerNight: Number,
  propertyType: { type: String, enum: ["Apartment", "Villa", "Studio", "House"] },
  amenities: [String],
  images: [String], // Stored URLs (Cloudinary or local)
  host: { type: ObjectId, ref: "User" },
  isAvailable: { type: Boolean, default: true },
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
}


Relations:

Linked to a host (User)

Can have multiple bookings

3. Bookings Collection

Tracks all rental reservations made by users.

{
  _id: ObjectId,
  user: { type: ObjectId, ref: "User" },
  property: { type: ObjectId, ref: "Property" },
  checkInDate: Date,
  checkOutDate: Date,
  totalPrice: Number,
  status: { type: String, enum: ["Pending", "Confirmed", "Cancelled"], default: "Pending" },
  createdAt: { type: Date, default: Date.now }
}


Relations:

Connected to both User and Property collections

4. Images Collection (Optional)

Used when storing image metadata separately (for Cloudinary or external storage).

{
  _id: ObjectId,
  property: { type: ObjectId, ref: "Property" },
  url: String,
  public_id: String,
  uploadedAt: { type: Date, default: Date.now }
}

5. Reviews Collection (Future Enhancement)

For user feedback on properties.

{
  _id: ObjectId,
  property: { type: ObjectId, ref: "Property" },
  user: { type: ObjectId, ref: "User" },
  rating: { type: Number, min: 1, max: 5 },
  comment: String,
  createdAt: { type: Date, default: Date.now }
}

🔗 Relationships Diagram (Conceptual)
User (role: user/host/admin)
   ├── [1-to-many] → Bookings
   └── [1-to-many] → Properties (if Host)

Property
   ├── [1-to-many] → Bookings
   └── [1-to-many] → Images
       └── [optional] → Reviews

Booking
   ├── [1] → User
   └── [1] → Property

Indexes & Optimization

Indexed email in Users for fast login queries

Indexed location and pricePerNight in Properties for filtering

Compound index { property, checkInDate, checkOutDate } in Bookings to prevent date conflicts
