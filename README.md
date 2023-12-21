   private String createRequestBody(String ramlData, String message, String sha) throws JsonProcessingException {
        String encodedContent = Base64.getEncoder().encodeToString(ramlData.getBytes());
        Map<String, Object> requestBodyMap = new HashMap<>();
        requestBodyMap.put("message", message);
        requestBodyMap.put("content", encodedContent);
        if (sha != null) {
            requestBodyMap.put("sha", sha);
        }
        return objectMapper.writeValueAsString(requestBodyMap);
    }
