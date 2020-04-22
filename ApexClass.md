'''
### Apex Class
'''
global class UpdateContactAddresses implements Database.Batchable<sObject>, Database.Stateful {
    
    global Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator(
            'SELECT * FROM Lead WHERE Country="US"' 
        );
    }

    global void execute(Database.BatchableContext bc, List<Lead> scope)
    {
        for (Lead Leads : scope)
        {
            Leads.LeadSource = 'Web';
        }
        update scope;
    }   

    global void execute(Database.BatchableContext bc, List<Lead> Rating)
    {
        for (Lead Leads : Rating)
        {
            Leads.Rating = 'Hot';
        }
        update Rating;
    }   
    global void finish(Database.BatchableContext bc){   }
}

'''
### Apex Test Class
'''
@isTest
public class LeadProcessorTest
{
  static testMethod void testMethod1()
  {
  
      List<Lead> lstLead = new List<Lead>();
      for(Integer i=0 ;i <200;i++)
      {
        Lead led = new Lead();
        led.FirstName ='FirstName';
        led.LastName ='LastName'+i;
        led.Company ='demo'+i;
        lstLead.add(led);
      }
      insert lstLead;
      Test.startTest();
      LeadProcessor obj = new LeadProcessor();
      DataBase.executeBatch(obj);
      Test.stopTest();
  }

