# Flask-Principal-


```python
# we must have position as we defined it  in @identity_loaded.connect_via which have the role (you can change position to anything)


from flask_principal import Principal, Permission, UserNeed, RoleNeed,identity_loaded, AnonymousIdentity, identity_changed


principals = Principal(app)

# create role premssion
subscriber_permission = Permission(RoleNeed('subscriber'))


# search for position
    @identity_loaded.connect_via(app)
    def on_identity_loaded(sender, identity):
        # Set the identity user object
        identity.user = current_user
        # Add the UserNeed to the identity
        if hasattr(current_user, 'id'):
            identity.provides.add(UserNeed(current_user.id))

        # Assuming the User model has a list of roles, update the
        # identity with the roles that the user provides
        if hasattr(current_user, 'position'):
            # for role in current_user.role:
            identity.provides.add(RoleNeed(str(current_user.position)))


# handle the error for premssion
    @app.errorhandler(403)
    def no_permission(e):
        # message to be shown for soical media
        # logged user if he need access profile
        flash('Nemate dozvolu za pristup ovoj stranici')
        return redirect(url_for('main.index'))

# add the premssion with code in endpoint

@ subscriber_permission.require(http_exception=403)
```

important links:
https://pythonhosted.org/Flask-Principal/
https://stackoverflow.com/questions/61939800/role-based-authorization-in-flask-login
https://stackoverflow.com/questions/20069150/flask-principal-best-practice-of-handling-permissiondenied-exception
