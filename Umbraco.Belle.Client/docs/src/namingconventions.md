#Naming conventions

##modules handle namespacing, 
Naming: last one wins
but to do this automaticly, we would need
to auto-register all modules needed on app-start.
This gives us a couple of complications for handling 3rd party applications. 
As it would be a pain to scan every file for modules to load on app start.

Proposed structure:
http://briantford.com/blog/huuuuuge-angular-apps.html

Register all modules in app.js
Root module: Umbraco
	- contains the core services: notifications,dialogs,etc
	- contains the core resources: content,media etc
	- contains the core directives

1 module pr file princible

1st level modules:
	Umbraco (for misc system-level stuff, dialogs, notifications, etc)
	Umbraco.PropertyEditors (for all editors, as they are shared between all modules)
		- how would a 3rd party property editor access a 3rd party service in the Ucommerce module, when its loaded in the content editor? (which is inside the the content module?)
	Umbraco.Content (registers that it needs umbraco module, for access to core services )
	Umbraco.Media
	Umbraco.Settings
	Ucommerce
	TeaCommerce
	Etc

Should all (core + 3rd party) services, filters and directives just be dependencies in the global umbraco module?
	

Each section namespace knows what modules to initially have loaded: 
Umbraco.Content
	- EditController
	- SortController

Inside the EditController module, it references the needed services:
	- DialogService
	- ContentFactory
	- Etc

So things are only loaded when needed, due to the modules, and our application, really only needs to know about the root modules, Content,Media,Settings, Ucommerce etc.

##Directives
If more directives have the same name, they will all be run

##Controllers
Last one wins, so you can override core controllers
Can be namedspaced, but shouldnt, the module is the namespace?
Module: Umbraco.Section.Area.Page
Name: PageNameController

Filename: /umbraco/section/area/pagename.controller.js
Ex: /umbraco/content/document/edit.controller.js
Ex: /umbraco/settings/documenttype/edit.controller.js

There is a need to have an extra level in the content tree urls due to all
other sections have this as well, and we dont want to confuse the routing.

The ctrl acronym is horrible and should not be used IMO.



##Services
typeService ? - cant be namespaced
Module: Umbraco.Services.Type
Name: TypeService?

Ex: Umbraco.Services.Notifications.NotificationsService
Filename: /umbraco/common/services/notifcations.service.js


##Resources
- Content, media, etc, cant be namespaced:
Module: Umbraco.Resources.Type
Name: TypeFactory?

Ex: Umbraco.Resources.Content.ContentFactory?
filename: /umbraco/common/resources/content.resources.js ? or
/umbraco/common/resoures/content.factory.js ?



