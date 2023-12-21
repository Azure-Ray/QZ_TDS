import org.springframework.http.*;
import org.springframework.web.client.HttpClientErrorException;
import org.springframework.web.client.RestTemplate;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;
import java.util.Base64;

@Service
public class GitService {

    private final RestTemplate restTemplate;

    @Value("${git.token}")
    private String gitToken;

    public GitService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public String saveRamlToGit(String ramlData, String fileName, String gitRepoUrl) {
        HttpHeaders headers = new HttpHeaders();
        headers.setBearerAuth(gitToken);
        headers.setContentType(MediaType.APPLICATION_JSON);

        // 构建文件在Git仓库的API URL
        String fileApiUrl = gitRepoUrl + "/contents/" + fileName;
        boolean fileExists;

        try {
            // 尝试获取文件
            restTemplate.exchange(fileApiUrl, HttpMethod.GET, new HttpEntity<>(headers), String.class);
            fileExists = true;
        } catch (HttpClientErrorException e) {
            // 如果文件不存在，则捕获404错误
            if (e.getStatusCode() == HttpStatus.NOT_FOUND) {
                fileExists = false;
            } else {
                // 其他错误应该被正常抛出
                throw e;
            }
        }

        // 创建或更新文件的请求体
        String commitMessage = fileExists ? "Update " : "Create ";
        String requestBody = createRequestBody(ramlData, commitMessage + fileName);
        HttpEntity<String> entity = new HttpEntity<>(requestBody, headers);

        // 提交文件更改
        restTemplate.exchange(
            fileApiUrl, HttpMethod.PUT, entity, String.class
        );

        // 构建文件在Git仓库的直接URL
        String fileDirectUrl = gitRepoUrl.replace("api.", "").replace("/repos", "") + "/blob/master/" + fileName;
        return fileDirectUrl;
    }

    private String createRequestBody(String ramlData, String message) {
        String encodedContent = Base64.getEncoder().encodeToString(ramlData.getBytes());
        return "{\"message\":\"" + message + "\",\"content\":\"" + encodedContent + "\"}";
    }
}
