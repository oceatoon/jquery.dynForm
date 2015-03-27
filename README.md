jquery.dynForm
------

light json based , schema oriented Form generator 
- jquery pluggin
- 100% javascript
- desciption based using json-schema.org
- Form validation
- bootstrap designed 
- 2 way dataBinding for Editing Forms (set form values)
- can into any Json structure

Sample 
------
this is a first use case 
I'll come back with a more detailed  presentation 

```
var formDefinition = {
	    "jsonSchema" : {
	        "title" : "Todyn Form",
	        "type" : "object",
	        "properties" : {
	        	"separator1":{
	        		"title":"Organization"
	        	},
	        	"parentOrganisation" : {
	                "inputType" : "text",
	                "value" : "<?php echo $_GET['id'] ?>",
	                "icon" : "fa fa-user",
	                "placeholder":"Your Name"
	            },
	            "organizationName" : {
	                "inputType" : "text",
	                "icon" : "fa fa-group",
	                "placeholder" : "Name of your Organization",
	                "rules" : {
						"required" : true
					}
	            },
	            "description" :{
	            	"inputType" : "textarea",
	            	"placeholder" : "Describe your Organization"
	            },
	            "type" :{
	            	"inputType" : "select",
	            	"placeholder" : "Describe your Organization",
	            	"options" : {
	            		"NGO":"NGO",
                    	"LocalBusiness":"Local Businesse",
                    	"Groups":"Group"
	            	}
	            },
	            "postalCode" : {
	                "inputType" : "text",
	                "icon" : "fa fa-map-marker",
	                "placeholder":"Postal Code",
	                "rules" : {
						"required" : true
					}
	            },
	            "organizationEmail" : {
	                "inputType" : "text",
	                "icon" : "fa fa-envelope",
	                "placeholder":"Organization Email"
	            },
	            "separator2":{
	        		"title":"Person"
	        	},
	            "personName" : {
	                "inputType" : "text",
	                "icon" : "fa fa-user",
	                "placeholder":"Your Name"
	            },
	            "personEmail" : {
	                "inputType" : "text",
	                "icon" : "fa fa-envelope",
	                "placeholder":"Your Email"
	            },
	            "isOrgaAdmin" : {
	                "inputType" : "checkbox",
	                "placeholder" : "I am the main contact of this organization",
	                "class":"grey"
	            }
	        }
	    },
	    "collection" : "todos",
	    "key" : "todoForm",
	    //"savePath":moduleId+""
	};
var organizationInitData = {
	"organizationEmail" : "toto@gogo.com",
	"organizationName":"PING PONG",
	"address": {
		"postalCode":"97400"
	}
};
var dataBindOrganization = {
	   "parentOrganisation": {"value":"<?php echo $_GET['id'] ?>"},
	   "#organizationEmail" : "organizationEmail" , 
       "#organizationName" : "organizationName",
       "#postalCode" : "address.postalCode",
       "#organizationCountry" : {"value":"Réunion"},
       "#type" : "type",
       "#description" : "description",
       "#personName" : "personName",
       "#personEmail" : "personEmail"
};

jQuery(document).ready(function() {
	$('.box-join').show().addClass("animated flipInX").on('webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend', function() {
		$(this).removeClass("animated flipInX");
	});
	var form = $.dynForm({
		formId : "#form-join",
		formObj : formDefinition,
		onLoad : function  () {
			/*
			here you can load anythnig into your form fields 
			it is called after creation
			*/
			$.each(dataBindOrganization,function(field,path){
					console.log("field key",field);
					if(field != "")
						$(field).val(jsonHelper.getValueByPath( organizationInitData, path ));
				});
			
		},
		onSave : function(){
			console.log("saving Organization!!");
			var organization = {};
			$.each(dataBindOrganization,function(field,path){
				console.log("save key ",field,path);
				if(field != "" )
				{
					if( $(field) && $(field).val() && $(field).val() != "" )
					{
						value = $(field).val();
						jsonHelper.setValueByPath( organization, path, value );
					} 
				}
				else
					console.log("save Error",field);
			});
			console.dir(organization);
			$.unblockUI();
			return false;
			/*var params = { 
			   "parentOrganisation":"<?php echo $_GET['id'] ?>",
			   "organizationEmail" : $("#organizationEmail").val() , 
               "organizationName" : $("#organizationName").val(),
               "postalCode" : $("#postalCode").val(),
               "organizationCountry" : "Réunion",
               "type" : $("#type").val(),
               "description" : $("#description").val(),
               "personName" : $("#personName").val(),
               "personEmail" : $("#personEmail").val()
            };
		      
	    	$.ajax({
	    	  type: "POST",
	    	  url: baseUrl+"/<?php echo $this->module->id?>/organization/saveNewAddMember",
	    	  data: params,
	    	  dataType: "json"
	    	}).done(function(data){
	    		if(data.result)
	        		window.location.reload();
	    		else 
	    		{
	    			$.unblockUI();
					$('.errorHandler').html(data.msg);
					$('.errorHandler').show();
	    		}
	    	});*/
		}
	});
	console.dir(form);
});
```

Stuff we used , and are thankfull to
- jquery 
- bootstrap 
- toastr
- blockui
- jquery-validation/dist/jquery.validate.min.js
- bootstrap-datepicker.js
- https://github.com/oceatoon/jsonHelpers
