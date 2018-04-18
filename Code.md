    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
    public class AdministratorsOnly : AuthorizeAttribute
    {
        public AdministratorsOnly() { }

        public override void OnAuthorization(AuthorizationContext filterContext)
        {
            var personLoggedIn = filterContext.HttpContext.User.Identity.Name.Split('\\')[1];
            using (var _context = new ConnectionStringName())
            {
                if (_context.TableName.Single(x => x.PropertyOne == personLoggedIn).IsAdministrator == false)
                {
                    filterContext.Result = new ViewResult
                    {
                        ViewName = "NotAuthorized"
                    };
                }

            }

            base.OnAuthorization(filterContext);

        }
    }
