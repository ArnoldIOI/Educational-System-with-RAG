# Competency Questions
## 1 How does the prompts of the student evolve?
PREFIX rag: <http://example.org/rag-based-edu-ontology#>
PREFIX p-plan: <http://purl.org/net/p-plan#>
PREFIX prov: <http://www.w3.org/ns/prov#>

SELECT ?solution ?chatSession ?prompt ?student
WHERE {
  ?solution a rag:StudentSolution ;
            rag:BasedOnChatSession ?chatSession .
  ?chatSession rag:ChatSessionStartedByStudent ?student ;
               rag:id ?chatSessionID .
  ?round p-plan:isStepOfPlan ?chatSession ;
         p-plan:isDecomposedAsPlan ?plan .
  ?plan p-plan:isSubPlanOfPlan ?chatSession .
  ?generation p-plan:isStepOfPlan ?plan ;
              p-plan:hasInputVar ?prompt .
  ?prompt a rag:Prompt .
  ?solution rag:id "1718191203" .
}

## 2 Are two students’ prompts similar?
PREFIX rag: <http://example.org/rag-based-edu-ontology#>
PREFIX p-plan: <http://purl.org/net/p-plan#>

SELECT ?student ?query ?queryContent
WHERE {
  ?chatSession a rag:ChatSession ;
               rag:ChatSessionStartedByStudent ?student ;
               rag:id ?chatSessionID .
  ?round p-plan:isStepOfPlan ?chatSession ;
         p-plan:isDecomposedAsPlan ?plan .
  ?plan p-plan:isSubPlanOfPlan ?chatSession .
  ?retrieval p-plan:isStepOfPlan ?plan ;
             p-plan:hasInputVar ?query .
  ?query a rag:Query ;
         rag:queryContent ?queryContent .
}
ORDER BY ?student ?query

## 3 Does the student cite the System’s response properly?
PREFIX rag: <http://example.org/rag-based-edu-ontology#>
PREFIX p-plan: <http://purl.org/net/p-plan#>

SELECT ?student ?solution ?response ?responseContent
WHERE {
  ?solution a rag:StudentSolution ;
            rag:id "1718191203" ;
            rag:BasedOnChatSession ?chatSession .
  ?chatSession a rag:ChatSession ;
               rag:ChatSessionStartedByStudent ?student ;
               rag:id ?chatSessionID .
  ?round a rag:Round ;
         p-plan:isStepOfPlan ?chatSession ;
         p-plan:isDecomposedAsPlan ?plan .
  ?generation a rag:Generation ;
              p-plan:isStepOfPlan ?plan ;
              p-plan:hasOutputVar ?response .
  ?response a rag:Response ;
            rag:responseContent ?responseContent .
}
ORDER BY ?student ?solution ?response

## 4 Does the student use the response properly?
PREFIX rag: <http://example.org/rag-based-edu-ontology#>
PREFIX p-plan: <http://purl.org/net/p-plan#>

SELECT ?student ?query ?queryContent ?response ?responseContent
WHERE {
  ?student rag:id "222333" .
  ?chatSession a rag:ChatSession ;
               rag:ChatSessionStartedByStudent ?student ;
               rag:id ?chatSessionID .
  ?round a rag:Round ;
         p-plan:isStepOfPlan ?chatSession ;
         p-plan:isDecomposedAsPlan ?plan .
  ?retrieval a rag:Retrieval ;
             p-plan:isStepOfPlan ?plan ;
             p-plan:hasInputVar ?query .
  ?query a rag:Query ;
         rag:queryContent ?queryContent .
  ?generation a rag:Generation ;
              p-plan:isStepOfPlan ?plan ;
              p-plan:hasOutputVar ?response .
  ?response a rag:Response ;
            rag:responseContent ?responseContent .
}
ORDER BY ?student ?query


## 5 Is the information retrieved correct and related to the query?
PREFIX rag: <http://example.org/rag-based-edu-ontology#>
PREFIX p-plan: <http://purl.org/net/p-plan#>
PREFIX prov: <http://www.w3.org/ns/prov#>

SELECT ?round ?retrievedInfo ?query
WHERE {
  ?round a rag:Round ;
         p-plan:isDecomposedAsPlan ?plan .
  ?retrieval p-plan:isStepOfPlan ?plan ;
             p-plan:hasOutputVar ?retrievedInfo ;
             p-plan:hasInputVar ?query .
  ?retrievedInfo a rag:RetrievedInformation .
  ?query a rag:Query .
}
