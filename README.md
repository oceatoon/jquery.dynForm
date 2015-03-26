# dynForm
generate Forms jquery pluggin, javascript , validation, bootstrap, json schema definition

Sample 
------
this is a first use case 
I'll come back with a more detailed  presentation 

'''
var form = $.dynForm({
		formId : "#form-join",
		formObj : {
		    "jsonSchema" : {
		        "title" : "Todyn Form",
		        "type" : "object",
		        "properties" : {
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
		                "placeholder":"Postal Code"
		            },
		            "organizationEmail" : {
		                "inputType" : "text",
		                "icon" : "fa fa-envelope",
		                "placeholder":"Organization Email"
		            },
		            "namePerson" : {
		                "inputType" : "text",
		                "icon" : "fa fa-user",
		                "placeholder":"Your Name"
		            },
		            "parentOrganisation" : {
		                "inputType" : "hidden",
		                "value" : "<?php echo $_GET['id'] ?>",
		                "icon" : "fa fa-user",
		                "placeholder":"Your Name"
		            },
		            "date" : {
		                "inputType" : "date",
		                "icon" : "fa fa-calendar",
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
		},
		onSave:function(){

			var params = { 
			   "namePerson" : $("#namePerson").val() ,
			   "organizationEmail" : $("#organizationEmail").val() , 
               "organizationName" : $("#organizationName").val(),
               "postalCode" : $("#postalCode").val(),
               "organizationCountry" : "Réunion",
               "type" : $("#typeOrga").val(),
               "description" : $("#description").val(),
               "parentOrganisation":"<?php echo $_GET['id'] ?>"
            };
		      
	    	$.ajax({
	    	  type: "POST",
	    	  url: baseUrl+"/<?php echo $this->module->id?>/organization/saveNewAddMember",
	    	  data: params,
	    	  success: function(data){
	    		  if(data.result)
	    		  {
	        		window.location.reload();
	    		  }
	    		  else {
					$('.registerResult').html(data.msg);
					$('.registerResult').show();
	    		  }
	    	  },
	    	  dataType: "json"
	    	});
		}
	});
	'''

Stuff we used , and are thankfull to
- jquery 
- bootstrap 
- toastr
- blockui
- jquery-validation/dist/jquery.validate.min.js
- bootstrap-datepicker.js
