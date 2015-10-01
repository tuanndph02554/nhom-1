# nhom-1 flksdfjksdf
/**
 * Creates an authorized Bigquery client service using Application Default Credentials.
 *
 * @return an authorized Bigquery client
 * @throws IOException if there's an error getting the default credentials.
 */
public static Bigquery createAuthorizedClient() throws IOException {
  // Create the credential
  HttpTransport transport = new NetHttpTransport();
  JsonFactory jsonFactory = new JacksonFactory();
  GoogleCredential credential = GoogleCredential.getApplicationDefault(transport, jsonFactory);

  // Depending on the environment that provides the default credentials (e.g. Compute Engine, App
  // Engine), the credentials may require us to specify the scopes we need explicitly.
  // Check for this case, and inject the Bigquery scope if required.
  if (credential.createScopedRequired()) {
    credential = credential.createScoped(BigqueryScopes.all());
  }

  return new Bigquery.Builder(transport, jsonFactory, credential)
      .setApplicationName("Bigquery Samples").build();
}
