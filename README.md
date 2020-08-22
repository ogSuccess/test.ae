# COMPLETE GUIDE TO THE TUTORIAL AND STEPS
# SEE SOPHIA DOCUMENTATION FOR FURTHER LEARNING
https://github.com/aeternity/aesophia/blob/lima/docs/sophia.md

# THEORETICAL TUTORIALS 
https://github.com/aeternity/protocol/blob/master/contracts/contracts.md

 1 MODEL THE FILE OBJECT

ID : INTEGER
NAME: STRING
DESCRIPTION : STRING
CREATEDAT : INT
UPDATEDAT: INT
OWNER: ADDRESS
HARSH : STRING

2: CREATE A ADD FILE FUNCTIONALITY
3: RETRIEVE THE FILES


// 1.
 contract File = 
    record file = {
        id: int,
        name: string,
        description: string,
        createdAt: int,
        updatedAt: int,
        owner: address,
        harsh: string}

    /*CREATE A STATE WHUCH WILL KEEP TRACK OF EVERYTHING WE DO ON THE SMART CONTRACT

    LIKE MODIFICATIONS, ADDITIONS, 

    HERE ALSO WE INPUT VALUES THAT MIGHT CHANGE DURING THE WRITING OF A SMART CONTRACT
    WHICH IS A RECORD, HERE WE USE THE MUTABLE "STATE", AN EXAMPLE OF AN IMMUTABLE STATE
    IS THE BALANCE TYPE.
    SO A STATE IS A REDORD OF MUTABLE VALUES TYPE THAT MAY CHANGE IN A SMART CONTRACT.

    CREATE A KEY CALLED INDEX COUNTER ASSIGN AN INT VALUE TO IT AND CALL OUR FILES
    */  


    record state ={ 
        index_counter: int,
        files:map(int, file)} // A DICTIONARY OR AN ARRAY, TAKE THE FILE AND ASSIGN THE VALUE TO AN INTEGER

    /*
        FUNCTIONS IN AETERNITY 
        AETERNITY USES MODIFIERS AS A WAY OF STATING FUNCTIONS
        LIKE PRIVATE AND PUBLIC, A PUBLIC FUNCTION IN AETERNITY IS CALLED AN
        ENTRYPOINT, WHICH GIVES ROOM FOR TESTING
        SO WE WILL DEFINE A CONSTRUCTOR FUNCTION 
        WHICH IS WILL BE AN INIT FUNCTION WHICH IS CALLED
        WHEN INITIALIZING YOUR SMART CONTRACT
    
    
     */
    entrypoint init() = {
      index_counter = 0, // TRACK THE NUMBER OF FILES SAVED
      files={}} // CREATE NEW FILE RECORD, WE MAY UPLOAD MORE THAN ONE IMAGES.

      /* 
       CREATE AN ENTRY POINT FUNCTION TO GET THE LENGHT OF FILES
       IN OUR SMART CONTRACT
      */

    entrypoint getFileLength(): int = 
    // RETURNS THE TOTAL NUMBER OF FILES SAVED IN OUR SMART CONTRACT
        state.index_counter

        /*  
        THIS FUNCTION TELLS THE COMPLILER IT MODIFIERS THE STATE OF YOUR CONTRACT
    2: CREATE A ADD FILE FUNCTIONALITY

        */
    stateful entrypoint add_file (_name:string, _description:string, _harsh:string)=

     let stored_file={
         id= getFileLength() + 1,
         name = _name,
         description =_description,
         createdAt= Chain.timestamp, // HERE IT MEANS THE INITIAL TIME THE BLOCK IS CREATED
         updatedAt = Chain.timestamp,
         owner = Call.caller, // THE ADDRESS OF THE CALLER.
         harsh = _harsh}

     let index = getFileLength() + 1
     put(state{files[index]=stored_file, index_counter = index})

     /* 
     3: RETRIEVE THE FILES
     map.lookup if found return if not found trigger the abort statement.
     */

    entrypoint get_file_by_index(index:int) : file =
        switch(Map.lookup(index, state.files))
         none => abort( "file does not exist with this index")
         Some(x) => x 
        







