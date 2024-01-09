package com.yourpackage.service;

import com.yourpackage.model.ExtraCrMap;
import com.yourpackage.repository.ExtraCrMapRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.time.LocalDateTime;
import java.util.Optional;

@Service
public class ExtraCrMapService {

    @Autowired
    private ExtraCrMapRepository extraCrMapRepository;

    @Transactional
    public ExtraCrMap upsertExtraCrMap(ExtraCrMap newExtraCrMap) {
        Optional<ExtraCrMap> existingExtraCrMap = extraCrMapRepository.findByCrNumber(newExtraCrMap.getCrNumber());

        if (existingExtraCrMap.isPresent()) {
            ExtraCrMap updatedExtraCrMap = existingExtraCrMap.get();
            updateNonNullFields(updatedExtraCrMap, newExtraCrMap);
            updatedExtraCrMap.setUpdatedAt(LocalDateTime.now());
            return extraCrMapRepository.save(updatedExtraCrMap);
        } else {
            newExtraCrMap.setCreatedAt(LocalDateTime.now());
            newExtraCrMap.setUpdatedAt(LocalDateTime.now());
            return extraCrMapRepository.save(newExtraCrMap);
        }
    }

    private void updateNonNullFields(ExtraCrMap existing, ExtraCrMap newExtraCrMap) {
        if (newExtraCrMap.getCyberflowScanUrl() != null) {
            existing.setCyberflowScanUrl(newExtraCrMap.getCyberflowScanUrl());
        }
        // 类似地为其他字段添加检查和更新
        // ...
    }
}
