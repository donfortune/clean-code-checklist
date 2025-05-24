
CLEAN CODE SAFE Checklists
=========================

C - Consistency
---------------

Follow a consistent code style (e.g., Prettier, ESLint)

Use clear naming conventions for variables, functions, and files

### ✅ Good Example

const getUserProfile = (userId) => {
    return db.findById(userId);
};
    

### ❌ Bad Example:

const getusr = (uid) => db.fnd(uid);
    

L - Logging
-----------

Use proper logging libraries like Winston or Pino

Avoid logging sensitive data

### ✅ Good Example:

const logger = require('winston');
logger.info(\`User ${userId} logged in\`);
    

### ❌ Bad Example:

console.log(\`Password for ${userId}: ${password}\`);
    

E - Error Handling
------------------

Implement centralized error handling middleware

### ✅ Good Example:

app.use((err, req, res, next) => {
    res.status(500).json({ error: err.message });
});
    

### ❌ Bad Example:

app.get('/user', (req, res) => {
    throw new Error('User not found'); // No proper handling
});
    

A - Authentication & Authorization
----------------------------------

Use JWT for secure token-based authentication

### ✅ Good Example:

const jwt = require('jsonwebtoken');
const token = jwt.sign({ id: userId }, process.env.JWT\_SECRET, { expiresIn: '1h' });
    

### ❌ Bad Example:

const token = Buffer.from(\`user:${userId}\`).toString('base64'); // Insecure encoding
    

N - Normalization
-----------------

Keep database schemas clear and concise

### ✅ Good Example:

const userSchema = new mongoose.Schema({
    name: String,
    email: { type: String, unique: true },
});
    

### ❌ Bad Example:

const userSchema = new mongoose.Schema({
    userName: String,
    userEmail: String, // No unique constraint
});
    

C - Caching
-----------

Use caching tools like Redis

### ✅ Good Example:

const cacheData = await redis.get('user:123');
if (cacheData) return JSON.parse(cacheData);
    

### ❌ Bad Example:

const data = await db.findById(123); // No caching
    

O - Optimization
----------------

Use pagination, filtering, and indexing for performance

### ✅ Good Example:

const users = await db.find().limit(10).skip(20);
    

### ❌ Bad Example:

const users = await db.find(); // Large data load without pagination
    

D - Data Validation
-------------------

Use libraries like Joi or express-validator

### ✅ Good Example:

const schema = Joi.object({
    email: Joi.string().email().required(),
});
    

### ❌ Bad Example:

if (!req.body.email.includes('@')) return res.status(400).send('Invalid email');
    

E - Environment Management
--------------------------

Use .env files for sensitive information

### ✅ Good Example:

DB\_PASSWORD=securepassword123
JWT\_SECRET=mystrongsecret
    

### ❌ Bad Example:

const password = 'securepassword123'; // Hardcoded
    

S.A.F.E - Security Checklist
----------------------------

### S - Sanitize Inputs

*   Use libraries like \`express-validator\` to clean data before processing.

**✅ Good Example:**  
`const { body } = require('express-validator');   router.post('/login', body('email').isEmail(), handler);`

**❌ Bad Example:**  
`router.post('/login', (req, res) => { const email = req.body.email; });`

### A - Authenticate Requests

*   Use JWT for secure token-based authentication.

**✅ Good Example:**  
`const token = jwt.sign({ id: user.id }, process.env.JWT_SECRET);`

**❌ Bad Example:**  
`const token = jwt.sign({ id: user.id }, 'insecure-key');`

### F - Filter User Permissions

*   Implement Role-Based Access Control (RBAC).

**✅ Good Example:**  
`if (user.role === 'admin') { // grant access }`

**❌ Bad Example:**  
`if (user) { // assumes all users have admin access }`

### E - Encrypt Sensitive Data

*   Use libraries like \`bcrypt\` to hash passwords.

**✅ Good Example:**  
`const hashedPassword = await bcrypt.hash(password, 10);`

**❌ Bad Example:**  
`const password = req.body.password; // Stored as plain text`
