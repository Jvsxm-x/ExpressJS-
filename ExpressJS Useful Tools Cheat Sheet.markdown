# ExpressJS Useful Tools Cheat Sheet

## Middleware
- **Morgan**: HTTP request logger middleware
  ```javascript
  const morgan = require('morgan');
  app.use(morgan('dev'));
  ```
- **CORS**: Enable Cross-Origin Resource Sharing
  ```javascript
  const cors = require('cors');
  app.use(cors());
  ```
- **Helmet**: Secure Express apps by setting HTTP headers
  ```javascript
  const helmet = require('helmet');
  app.use(helmet());
  ```
- **Body-Parser**: Parse incoming request bodies
  ```javascript
  const bodyParser = require('body-parser');
  app.use(bodyParser.json());
  ```

## Error Handling
- **Express-Async-Errors**: Handle async/await errors
  ```javascript
  require('express-async-errors');
  // Use with async route handlers
  app.get('/', async (req, res) => {
    throw new Error('Async error');
  });
  ```
- **Custom Error Middleware**:
  ```javascript
  app.use((err, req, res, next) => {
    res.status(500).json({ error: err.message });
  });
  ```

## Validation
- **Express-Validator**: Validate and sanitize input
  ```javascript
  const { check, validationResult } = require('express-validator');
  app.post('/user', [
    check('email').isEmail(),
    check('password').isLength({ min: 6 })
  ], (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    // Proceed with valid data
  });
  ```

## Authentication
- **Passport**: Authentication middleware
  ```javascript
  const passport = require('passport');
  const LocalStrategy = require('passport-local').Strategy;
  app.use(passport.initialize());
  passport.use(new LocalStrategy((username, password, done) => {
    // Authentication logic
  }));
  ```
- **JSON Web Tokens (jsonwebtoken)**:
  ```javascript
  const jwt = require('jsonwebtoken');
  const token = jwt.sign({ id: user.id }, 'secret', { expiresIn: '1h' });
  ```

## File Upload
- **Multer**: Handle multipart/form-data for file uploads
  ```javascript
  const multer = require('multer');
  const upload = multer({ dest: 'uploads/' });
  app.post('/upload', upload.single('file'), (req, res) => {
    res.send('File uploaded');
  });
  ```

## Templating
- **EJS**: Embedded JavaScript templating
  ```javascript
  const ejs = require('ejs');
  app.set('view engine', 'ejs');
  app.get('/', (req, res) => {
    res.render('index', { title: 'Home' });
  });
  ```
- **Pug**: Template engine
  ```javascript
  app.set('view engine', 'pug');
  app.get('/', (req, res) => {
    res.render('index', { title: 'Home' });
  });
  ```

## Database Integration
- **Mongoose** (MongoDB):
  ```javascript
  const mongoose = require('mongoose');
  mongoose.connect('mongodb://localhost/mydb');
  const User = mongoose.model('User', { name: String });
  ```
- **Sequelize** (SQL databases):
  ```javascript
  const { Sequelize } = require('sequelize');
  const sequelize = new Sequelize('sqlite::memory:');
  const User = sequelize.define('User', { name: Sequelize.STRING });
  ```

## API Documentation
- **Swagger-UI-Express**: API documentation
  ```javascript
  const swaggerUi = require('swagger-ui-express');
  const swaggerDocument = require('./swagger.json');
  app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));
  ```

## Testing
- **Supertest**: HTTP assertions for testing
  ```javascript
  const request = require('supertest');
  const app = require('./app');
  describe('GET /', () => {
    it('responds with json', async () => {
      await request(app)
        .get('/')
        .expect('Content-Type', /json/)
        .expect(200);
    });
  });
  ```
- **Mocha**: Test framework
  ```javascript
  const mocha = require('mocha');
  describe('Test Suite', () => {
    it('should run a test', () => {
      assert.equal(1, 1);
    });
  });
  ```

## Performance
- **Compression**: Compress response bodies
  ```javascript
  const compression = require('compression');
  app.use(compression());
  ```
- **Rate-Limit**: Rate limiting middleware
  ```javascript
  const rateLimit = require('express-rate-limit');
  app.use(rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100 // limit each IP to 100 requests
  }));
  ```

## Logging
- **Winston**: Versatile logging library
  ```javascript
  const winston = require('winston');
  const logger = winston.createLogger({
    transports: [
      new winston.transports.File({ filename: 'error.log', level: 'error' }),
      new winston.transports.File({ filename: 'combined.log' })
    ]
  });
  ```

## WebSockets
- **Socket.IO**: Real-time bidirectional communication
  ```javascript
  const socketIo = require('socket.io');
  const server = require('http').createServer(app);
  const io = socketIo(server);
  io.on('connection', (socket) => {
    socket.on('message', (msg) => {
      io.emit('message', msg);
    });
  });
  ```

## Environment Management
- **Dotenv**: Load environment variables
  ```javascript
  require('dotenv').config();
  const dbUri = process.env.DATABASE_URI;