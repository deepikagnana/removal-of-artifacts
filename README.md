# removal-of-artifacts
To perform artifact removal from biosignals using **Independent Component Analysis (ICA)** in MATLAB, you can use the `runica` function from the **EEGLAB** toolbox or the `fastica` function for ICA. Here's an example code to demonstrate the process of artifact removal from biosignals like EEG or ECG using ICA.

### Prerequisites:
1. Make sure you have **EEGLAB** or **FastICA** installed in your MATLAB environment.
2. The biosignal data should be in matrix form where each row corresponds to a channel, and each column corresponds to a time sample.

Here's the MATLAB code using **FastICA** for artifact removal:

### Example: Artifact Removal from EEG Signal using ICA

```matlab
% Load your biosignal data (e.g., EEG data) into the workspace
% For demonstration, assume `data` is a matrix where rows are channels and columns are time samples
% Replace this with loading your actual data (e.g., from a .mat file or CSV)

% Example: Simulated data (10 channels, 1000 samples)
% Replace this with actual EEG or biosignal data
data = randn(10, 1000);  % Example random data for 10 channels and 1000 samples

% 1. Preprocess the data (e.g., zero-mean, standardize)
data_centered = data - mean(data, 2);  % Centering the data (mean = 0)

% 2. Apply ICA using the FastICA algorithm
% FastICA requires the input data to be centered (already done above)
% Apply ICA to decompose the data into independent components

% Perform ICA (assuming you have 10 independent sources for 10 channels)
[icasig, A, W] = fastica(data_centered, 'numOfIC', 10);

% 3. Visualize the original and ICA components to identify artifacts
% Plot the original biosignal data (e.g., EEG channels)
figure;
subplot(2, 1, 1);
plot(data_centered');
title('Original Biosignal Data (Channels)');
xlabel('Time Samples');
ylabel('Amplitude');

% Plot the independent components after ICA
subplot(2, 1, 2);
plot(icasig');
title('Independent Components after ICA');
xlabel('Time Samples');
ylabel('Amplitude');

% 4. Manually identify the component corresponding to artifact
% You may need to visually inspect the components or use additional criteria
% (e.g., frequency analysis) to identify the artifact component.
% Let's assume artifact component is 1 (this should be identified through visualization)

% 5. Remove the artifact component
artifact_component = 1;  % Assume component 1 is the artifact
icasig_clean = icasig;
icasig_clean(artifact_component, :) = 0;  % Remove artifact component

% 6. Reconstruct the cleaned signal
data_cleaned = A * icasig_clean;  % Reconstruct the data without the artifact

% 7. Visualize the cleaned biosignal data
figure;
plot(data_cleaned');
title('Cleaned Biosignal Data (Artifact Removed)');
xlabel('Time Samples');
ylabel('Amplitude');

% Save the cleaned data if needed
% save('cleaned_data.mat', 'data_cleaned');
```

### Key Steps Explained:
1. **Data Centering**: Before performing ICA, the data is usually centered (subtracting the mean from each channel).
2. **FastICA**: The `fastica` function decomposes the mixed biosignal into independent components. The result is `icasig` (the independent components), `A` (mixing matrix), and `W` (unmixing matrix).
3. **Artifact Identification**: After ICA, you visually inspect the independent components (`icasig`). The artifact typically appears as one or more components with unusual patterns (such as high-frequency noise or large spikes). You can identify the component corresponding to the artifact.
4. **Removing Artifacts**: Once identified, the artifact component is removed by setting it to zero.
5. **Reconstruction**: The cleaned signal is reconstructed by multiplying the unmixing matrix `A` with the modified independent components.
6. **Visualization**: Finally, the cleaned signal is plotted to verify the artifact removal.

### Notes:
- **Artifact Identification**: Identifying the correct component to remove can be challenging and may require visual inspection, frequency analysis, or domain-specific knowledge (e.g., artifact components may correspond to muscle activity or eye movements in EEG data).
- **Number of Components**: You may need to adjust the number of components (`'numOfIC'`) based on your data. The more components you extract, the better resolution you'll have for separating artifacts.
- **EEGLAB**: If you're using EEGLAB, the process would be similar, and you could use the built-in ICA functions provided by EEGLAB (`pop_runica`).

