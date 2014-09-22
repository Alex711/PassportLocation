var passport = require('passport');

var Account = require('./models/account');

module.exports = function (app) {
    
  app.get('/', function (req, res) {
    
    if (req.user) {
     var searchID = req.user.id;
      console.log(searchID);
      
      //      Locations.find({ userid: req.user.id }, function(err, location) {

      Locations.find({name: "brisbane"}, function(err, location) {
  if (err) return console.error(err);
  
        res.render('index', {user : req.user, locations:location});
      });

// Connect to the db



      
    } else {
      
   
   

      res.render('index', { user : req.user });
       }
    

   

   
  });

  app.get('/register', function(req, res) {
      res.render('register', { });
  });

  app.post('/register', function(req, res) {
      Account.register(new Account({ username : req.body.username }), req.body.password, function(err, account) {
          if (err) {
            return res.render("register", {info: "Sorry. That username already exists. Try again."});
          }

          passport.authenticate('local')(req, res, function () {
            res.redirect('/');
          });
      });
  });
  
 

  app.get('/login', function(req, res) {
      res.render('login', { user : req.user });
  });

  app.post('/login', passport.authenticate('local'), function(req, res) {
      res.redirect('/');
  });

  app.get('/logout', function(req, res) {
      req.logout();
      res.redirect('/');
  });

  app.get('/ping', function(req, res){
      res.send("pong!", 200);
  });
  
};
