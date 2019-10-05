## How to generate exception in code(Rollback Exception):
Sometime for testing purpose the proof of concept we need to intentionally create an exception in code to recreate the rollback exception scenario in real life application. For example if an application is trying to save huge(100 of thousands) of data in database table and for some reason(Timeout/maintenance/external concurrent process) the saving process may get affected and there is high chance that it may get rollback. In such situation the process will not get completed and application wont get chance handle that properly. To handle such situation or to react such rollback exception we can apply intentional rollback exception with condition under exception clause and make appropriate db update so that user wont get confused and make sure exception properly handled. 

Below is an example code snippet of that.
    
    try{
	for(PrtSevAr psaobj: psalist){
		psa.setHsc(shcg);
		psaDao.set(psa);
	}
	if(ValidationTypes.SUCCESS == vr.getValidationType()){
		throw new RollbackException();
       }
    }catch(final Exception e){
	    uploadStatus = "Error";
	    hsCDao.saveUploadStatus(savedHsCId,uploadStatus);
	    LOG.error("Unable to save. "+e);
	    vr.setValidationType(ValidationTypes.Error);
	    vr.setCode("Falied");
	    return vr;
    }          

In above under if condition it throw new RollbackException intentionally for testing purpose.
